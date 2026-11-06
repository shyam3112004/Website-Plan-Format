<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 02 — Trd

# Technical Requirements Document (TRD)
**Related:** [PRD](./01-PRD.md) · [Backend Schema](./05-BACKEND-SCHEMA.md) · [Authentication](./06-AUTHENTICATION.md) · [Asset Pipeline](./07-ASSET-PIPELINE.md) · [Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md) · [Analytics Spec](./12-ANALYTICS-SPEC.md)

## 1. Architecture Overview

```
┌──────────────────────────── Browser ────────────────────────────┐
│  index.html (static, public)                                     │
│   ├─ main.js ── bootstraps modules                               │
│   ├─ contentLoader.js ── GET /api/page/:slug (fallback: JSON)    │
│   ├─ scrollCanvas.js ── preload frames → draw on <canvas>        │
│   ├─ overlays.js ──── GSAP timeline from content config          │
│   ├─ smoothScroll.js ─ Lenis ↔ ScrollTrigger                     │
│   ├─ leadForm.js ──── POST /api/leads                            │
│   └─ analytics.js ─── POST /api/events (sendBeacon)              │
│                                                                  │
│  admin.html (separate entry, noindex)                            │
│   ├─ admin/auth.js ── Supabase Auth sign-in only                 │
│   ├─ admin/guard.js ─ GET /api/admin/me, role routing            │
│   └─ admin/*.js ───── Bearer-token calls to /api/admin/*         │
└──────────────┬───────────────────────────────┬───────────────────┘
               │ static assets (frames, JS)    │ API calls
        ┌──────▼──────┐                 ┌──────▼───────────────────┐
        │ Vercel CDN  │                 │ Vercel Serverless        │
        │ /frames/*   │                 │ /api/page, /api/leads    │
        │ immutable   │                 │ /api/events              │
        └─────────────┘                 │ /api/admin/* (role-checked)
                                        └──────┬───────────────────┘
                                               │ service-role key
                                        ┌──────▼──────┐
                                        │  Supabase   │
                                        │  Postgres   │
                                        │  + Auth     │
                                        │  + Storage  │
                                        └─────────────┘
```

## 2. Tech Stack
| Layer | Choice | Reason |
|---|---|---|
| Build | Vite 5 (two entries: `index.html`, `admin.html`) | Fast dev server; admin code stays out of the public bundle |
| Language | Vanilla JS (ES2022 modules) | No framework overhead |
| Animation | GSAP 3 + ScrollTrigger | Industry standard, reliable scrub |
| Smooth scroll | Lenis | Lightweight, works with ScrollTrigger |
| Rendering | Canvas 2D + WebP/AVIF sequence | Smooth across browsers |
| Hosting | Vercel | CDN + serverless functions in one place |
| Database | Supabase Postgres | Managed, built-in dashboard for editors |
| Authentication | Supabase Auth (magic link + TOTP MFA) | Already in the stack; no password storage |
| Asset processing | FFmpeg | Frame extraction and compression |
| Analytics | Custom events table (+ optional Plausible) | Privacy-friendly |

## 3. Frontend Modules

### Public site
| Module | Responsibility | Public API |
|---|---|---|
| `contentLoader.js` | Fetch page config, fall back to `/content/home.json` | `loadContent(slug) → Promise<PageConfig>` |
| `scrollCanvas.js` | Preload, resize, render frames | `initScrollCanvas({ scene, onProgress, onComplete })` |
| `overlays.js` | Build overlay DOM and timeline | `initOverlays(overlays[])` |
| `smoothScroll.js` | Lenis setup | `initSmoothScroll() → lenis` |
| `leadForm.js` | Validate and submit leads | `initLeadForm(formEl)` |
| `analytics.js` | Track events, debounced | `track(name, props)` |

### Admin (loaded only by `admin.html`)
| Module | Responsibility | Public API |
|---|---|---|
| `admin/auth.js` | Sign-in, session, authenticated fetch | `signIn(email)`, `signOut()`, `apiFetch(path, init)` |
| `admin/guard.js` | Call `/api/admin/me`, route by role, handle `mfa_required` | `initGuard() → { role }` |
| `admin/mfa.js` | TOTP enroll and challenge | `enrollTotp()`, `verifyTotp(code)` |

### 3.1 PageConfig Contract
```ts
type PageConfig = {
  slug: string;
  title: string;
  scenes: Array<{
    id: string;
    scrollLengthVh: number;          // e.g. 400
    frames: {
      desktop: { basePath: string; count: number; ext: 'webp' | 'avif' };
      mobile:  { basePath: string; count: number; ext: 'webp' | 'avif' };
    };
    posterUrl: string;               // reduced-motion / noscript fallback
    overlays: Array<{
      id: string;
      start: number;                 // 0–1
      end: number;                   // 0–1
      heading: string;
      body?: string;
      cta?: { label: string; href: string };
      align: 'center' | 'left' | 'right';
    }>;
  }>;
};
```

The static file `/content/<slug>.json` also carries `metaDescription` and `ogImageUrl`. They are used at build time only, to fill the page's head tags ([SEO & Discoverability §3](./14-SEO-AND-DISCOVERABILITY.md)); the API response does not include them.

### 3.2 Content Loading With Fallback
```js
// src/contentLoader.js
export async function loadContent(slug = 'home') {
  try {
    const res = await fetch(`/api/page/${slug}`, { signal: AbortSignal.timeout(2500) });
    if (!res.ok) throw new Error(res.status);
    return await res.json();
  } catch {
    return (await fetch(`/content/${slug}.json`)).json();   // static fallback
  }
}
```

## 4. Rendering Pipeline
1. `loadContent()` picks the frame set (desktop or mobile) via `matchMedia('(max-width: 768px)')`.
2. Frames are preloaded in progressive order (every 4th → 2nd → all).
3. The loader hides once **keyframes (every 4th) are loaded**, not all frames. The rest load in the background.
4. ScrollTrigger maps `progress (0–1)` → `frameIndex = round(progress × (count − 1))`.
5. `render()` draws the nearest loaded frame. Canvas DPR is capped at 2.

## 5. Performance Budgets
| Asset | Desktop | Mobile |
|---|---|---|
| JS (gzipped), public bundle | ≤ 60 KB | ≤ 60 KB |
| CSS (gzipped), public bundle | ≤ 10 KB | ≤ 10 KB |
| Frames total | ≤ 12 MB (150 × ~80 KB) | ≤ 5 MB (120 × ~40 KB) |
| Time to interactive scrub | ≤ 3s on 4G | ≤ 4s on 4G |
| Main-thread work per frame | ≤ 8 ms | ≤ 12 ms |

The admin bundle (`admin.html`, includes `supabase-js`) is **not** counted against these budgets, and must never be imported by the public entry. Frame export and the audit that enforces the frame budgets are in the [Asset Pipeline](./07-ASSET-PIPELINE.md).

## 6. Browser Support
- Chrome/Edge/Firefox: last 2 versions
- Safari macOS 15+, iOS 15+
- No IE support. A `<noscript>` poster image is the fallback.

## 7. API Contract (summary; details in [Backend Schema](./05-BACKEND-SCHEMA.md) and [Authentication](./06-AUTHENTICATION.md) §4)
| Method | Endpoint | Auth | Purpose |
|---|---|---|---|
| GET | `/api/page/:slug` | Public | Published page config (cached 5 min) |
| POST | `/api/leads` | Public + rate limit | Create lead |
| POST | `/api/events` | Public + rate limit | Batch analytics events |
| GET | `/api/admin/me` | Editor | Current user and role |
| GET / PUT | `/api/admin/pages/:id` | Editor | Read or update a page (drafts included) |
| POST | `/api/admin/pages/:id/publish` | Editor | Publish a page |
| GET | `/api/admin/leads`, `/api/admin/leads.csv` | Admin (+ MFA) | View and export leads |
| DELETE | `/api/admin/leads/:id` | Admin (+ MFA) | Delete a lead |
| GET / POST / PATCH | `/api/admin/editors` | Admin (+ MFA) | List, invite, change or disable editors |
| GET | `/api/admin/audit` | Admin (+ MFA) | Audit log |

## 8. Security
- The Supabase **service-role key lives only in Vercel env vars**. The client never talks to the DB directly. The admin client uses Supabase JS **for sign-in only**.
- The Supabase **anon key is public by design** (it ships in the admin bundle). It is safe only because RLS denies by default (see Backend Schema §3).
- Leads: server-side validation, honeypot field, rate limit of 5/min per IP hash.
- IPs are stored only as a salted SHA-256 hash.
- A GDPR consent checkbox is required. The consent timestamp is stored.
- **Public pages** CSP: `default-src 'self'; img-src 'self' data:; connect-src 'self'`.
- **`/admin*`** CSP: same, plus `connect-src 'self' https://<project>.supabase.co`. Also `X-Robots-Tag: noindex, nofollow` and `Cache-Control: no-store`.
- Admin APIs use `Authorization: Bearer` headers, not cookies, so CSRF does not apply. CORS stays closed (same-origin only).
- No public sign-up. Every `/api/admin/*` route goes through one `requireRole` helper.

## 9. Environment Variables
| Name | Where | Description |
|---|---|---|
| `SUPABASE_URL` | Server | Project URL |
| `SUPABASE_SERVICE_ROLE_KEY` | Server | Server-only key. Never `VITE_`-prefixed. |
| `IP_HASH_SALT` | Server | Salt for IP hashing |
| `VITE_PAGE_SLUG` | Client | Default slug (`home`) |
| `SITE_URL` | Build | Public site URL for canonical and OG tags |
| `VITE_SUPABASE_URL` | Client (`/admin` only) | Project URL |
| `VITE_SUPABASE_ANON_KEY` | Client (`/admin` only) | Public anon key |

## 10. Error Handling
| Failure | Behavior |
|---|---|
| API down | Use static JSON fallback |
| Frame 404 | Skip it and render the nearest loaded frame |
| Frames slow (> 10s) | Show "Continue anyway"; enable scroll with partial frames |
| Lead submit fails | Inline error message, keep the form values |
| Admin call returns 401 | Try one silent token refresh; otherwise return to sign-in |
| Admin call returns 403 | Show "You don't have access"; for `mfa_required`, route to the MFA step |

## 11. Testing Strategy
| Type | Tool | Scope |
|---|---|---|
| Unit | Vitest | Frame index math, overlay timing, validation, `requireRole` (all role/MFA combinations) |
| Contract | Vitest + supertest | Every `/api/admin/*` route returns 401 without a token |
| E2E | Playwright | Loader → scroll → CTA → lead submit; magic-link sign-in → edit → publish; editor gets 403 on leads |
| Visual | Playwright screenshots at 0/25/50/75/100% scroll | Regressions |
| Performance | Lighthouse CI | Budgets in §5 |
| Manual | BrowserStack device matrix | iOS Safari, Android Chrome |

## 12. CI/CD
- GitHub → Vercel preview deploy on every PR
- GitHub Actions: lint → unit → build → Lighthouse CI (fails if budgets are exceeded)
- Merge to `main` → production deploy
- Release steps, smoke test and rollback: [Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md)

---
