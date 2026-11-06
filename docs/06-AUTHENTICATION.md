<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 06 — Authentication

# Authentication
**Related:** [PRD](./01-PRD.md) · [TRD](./02-TRD.md) · [App Flow](./03-APP-FLOW.md) · [Backend Schema](./05-BACKEND-SCHEMA.md) · [Implementation Plan](./17-IMPLEMENTATION-PLAN.md)

Requirements (FR-12 to FR-16, US-9 to US-12) live in the PRD, flow diagrams in App Flow §7, and the SQL in Backend Schema (`002_auth.sql`). This document holds the decisions, the API contract, the code and the checklists.

## 1. Scope and Decisions
The PRD lists **user accounts for visitors** as a non-goal, so visitors stay anonymous. Authentication covers **editors and admins**.

| Question | Decision |
|---|---|
| Who signs in? | Invited editors and admins only. No public sign-up. |
| Provider | Supabase Auth (already in the stack) |
| Method | Email magic link (OTP). TOTP MFA required for admins, optional for editors. |
| Session | Supabase JWT: access token 1h, refresh token rotated |
| Authorization | `editors` table (`editor` / `admin`), checked server-side on every request |
| DB access | Unchanged. The browser never queries Postgres. Supabase JS is used for sign-in only. |

### Roles
| Capability | Editor | Admin |
|---|---|---|
| Edit scenes and overlays (draft) | ✅ | ✅ |
| Publish / unpublish a page | ✅ | ✅ |
| View leads | ❌ | ✅ |
| Export leads (CSV) | ❌ | ✅ |
| Delete a lead (GDPR request) | ❌ | ✅ |
| Invite, disable, or change editors | ❌ | ✅ |
| View audit log | ❌ | ✅ |

## 2. API Contract
All routes require `Authorization: Bearer <access_token>`, return `Cache-Control: no-store`, and are same-origin only.

| Method | Endpoint | Role | Purpose |
|---|---|---|---|
| GET | `/api/admin/me` | editor | Current user and role |
| GET | `/api/admin/pages` | editor | List pages, including drafts |
| GET / PUT | `/api/admin/pages/:id` | editor | Read or update a page with its scenes and overlays |
| POST | `/api/admin/pages/:id/publish` | editor | Set `status = published`, `published_at = now()` |
| GET | `/api/admin/leads?cursor=` | admin | Paged leads |
| GET | `/api/admin/leads.csv` | admin | CSV export |
| DELETE | `/api/admin/leads/:id` | admin | Delete a lead |
| GET | `/api/admin/editors` | admin | List editors |
| POST | `/api/admin/editors` | admin | Invite `{ email, role }` |
| PATCH | `/api/admin/editors/:userId` | admin | Change role or `disabled` |
| GET | `/api/admin/audit?cursor=` | admin | Audit log |

### Errors
| Status | Body | When |
|---|---|---|
| 401 | `{ "error": "unauthenticated" }` | Missing, invalid or expired token |
| 403 | `{ "error": "forbidden" }` | Not an editor, disabled, or lacks the role |
| 403 | `{ "error": "mfa_required" }` | Admin route without an `aal2` session |
| 429 | `{ "error": "rate_limited" }` | More than 60 requests/min per user |

### Rules
- An admin cannot disable or demote themselves, and the last active admin cannot be removed.
- Every write calls `audit(actor, action, entity, id, diff)`.
- Bearer tokens in a header (not cookies) mean CSRF does not apply. Keep CORS closed.

## 3. Server Middleware
```js
// api/_lib/auth.js
import { createClient } from '@supabase/supabase-js';

const db = createClient(process.env.SUPABASE_URL, process.env.SUPABASE_SERVICE_ROLE_KEY);
const RANK = { editor: 1, admin: 2 };

export async function requireRole(req, res, minRole = 'editor') {
  res.setHeader('Cache-Control', 'no-store');

  const token = req.headers.authorization?.replace(/^Bearer /, '');
  if (!token) return deny(res, 401, 'unauthenticated');

  const { data, error } = await db.auth.getUser(token);      // verifies signature + expiry
  if (error || !data?.user) return deny(res, 401, 'unauthenticated');

  const { data: editor } = await db
    .from('editors').select('role, disabled').eq('user_id', data.user.id).maybeSingle();
  if (!editor || editor.disabled) return deny(res, 403, 'forbidden');
  if (RANK[editor.role] < RANK[minRole]) return deny(res, 403, 'forbidden');

  if (editor.role === 'admin') {                              // admins need MFA
    const claims = JSON.parse(Buffer.from(token.split('.')[1], 'base64url').toString());
    if (claims.aal !== 'aal2') return deny(res, 403, 'mfa_required');
  }
  return { user: data.user, role: editor.role, db };
}

function deny(res, status, error) { res.status(status).json({ error }); return null; }
```

Usage:
```js
// api/admin/leads.js
import { requireRole } from '../_lib/auth.js';

export default async function handler(req, res) {
  const ctx = await requireRole(req, res, 'admin');
  if (!ctx) return;
  const { data } = await ctx.db.from('leads').select('*')
    .order('created_at', { ascending: false }).limit(100);
  res.status(200).json(data);
}
```

## 4. Client
```js
// src/admin/auth.js  (loaded only by admin.html, so the public bundle stays within budget)
import { createClient } from '@supabase/supabase-js';

export const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY,
  { auth: { flowType: 'pkce', persistSession: true, autoRefreshToken: true, detectSessionInUrl: true } }
);

export const signIn = (email) =>
  supabase.auth.signInWithOtp({
    email,
    options: { shouldCreateUser: false, emailRedirectTo: `${location.origin}/admin` },
  });                                   // always show the same message, whatever the result

export const signOut = () => supabase.auth.signOut();

export async function apiFetch(path, init = {}) {
  const { data: { session } } = await supabase.auth.getSession();
  if (!session) throw new Error('unauthenticated');
  return fetch(path, {
    ...init,
    headers: { 'Content-Type': 'application/json', ...init.headers,
               Authorization: `Bearer ${session.access_token}` },
  });
}
```

## 5. Supabase Dashboard Settings
- Authentication → Providers → Email: **disable "Allow new users to sign up"**, enable magic link / OTP
- URL configuration: site URL = production domain; redirect allow-list = `https://<domain>/admin` plus the Vercel preview pattern
- JWT expiry: 3600 s; refresh token rotation on
- MFA: enable TOTP
- Bot protection: enable CAPTCHA (Turnstile) on auth requests
- Custom SMTP: send auth emails from your own domain ([Domain, DNS & Email §5.3](./13-DOMAIN-DNS-EMAIL.md)). The built-in sender is for testing only, so do this before inviting real editors.
- Invite editors with `supabase.auth.admin.inviteUserByEmail()` from `POST /api/admin/editors` (service role, server only)

## 6. Headers (`vercel.json`)
```json
{
  "headers": [{
    "source": "/admin(.*)",
    "headers": [
      { "key": "X-Robots-Tag", "value": "noindex, nofollow" },
      { "key": "Cache-Control", "value": "no-store" },
      { "key": "Content-Security-Policy",
        "value": "default-src 'self'; img-src 'self' data:; connect-src 'self' https://<project>.supabase.co" }
    ]
  }]
}
```
The public CSP is unchanged (`connect-src 'self'`). Only `/admin` may reach Supabase Auth.

## 7. Testing & Security Checks
| Type | Tool | Scope |
|---|---|---|
| Unit | Vitest | `requireRole`: no token, bad token, non-editor, disabled, editor on an admin route, admin without `aal2` |
| Contract | Vitest + supertest | Every `/api/admin/*` route returns 401 without a token |
| E2E | Playwright | Magic-link sign-in (Supabase test inbox), editor edits and publishes, editor gets 403 on leads, disabled editor is blocked |
| Manual | | Anon key cannot `select` from `leads`, `editors` or `audit_log` |
| Manual | | An unknown email shows the same message as a registered one |
| Manual | | `/admin` is absent from the sitemap and has `noindex` |
| Manual | | No `SUPABASE_SERVICE_ROLE_KEY` string in either built bundle |

## 8. Risks
| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Editor mailbox compromised, magic link stolen | Low | High | Short link expiry, MFA for admins, invite-only, audit log |
| Service-role key leak | Low | Critical | Server env only, never `VITE_`-prefixed, rotate on staff changes |
| Bug in one route skips the auth check | Medium | High | Single `requireRole` helper; contract test fails if any admin route lacks a 401 case |
| Offboarded editor keeps access | Medium | Medium | Disable in `editors` (blocked on the next request); tokens expire within 1h |
| Account enumeration | Low | Low | Generic sign-in message |

## 9. Where Auth Appears in the Other Docs
| Doc | Sections touched |
|---|---|
| 01-PRD | Non-goals, personas, US-9 to US-12, FR-11 to FR-16, auth NFR, scope, risks, release criteria |
| 02-TRD | Architecture, stack, admin modules, API contract, security, env vars, errors, tests |
| 03-APP-FLOW | Site map, §7 editor and admin flow, edge states |
| 04-UI-UX | Admin components, screens, accessibility, deliverables |
| 05-BACKEND-SCHEMA | ER diagram, `002_auth.sql`, RLS note, admin endpoints, retention |
| 17-IMPLEMENTATION-PLAN | Phase 8, Gantt, M6, risks, Definition of Done, §8 option |

---
