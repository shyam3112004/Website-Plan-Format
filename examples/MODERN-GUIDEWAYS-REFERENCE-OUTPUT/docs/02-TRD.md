# 02 — Technical Requirements Document (TRD)
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Architecture decision
Use a lightweight static-first application. The public site is built with Vite and vanilla JavaScript modules. GSAP ScrollTrigger maps scroll progress to frame indices. A Canvas 2D renderer displays the image sequence. Lenis is optional and must be disabled if it causes native-scroll or accessibility problems. Vercel serves the static site and API functions. Supabase is the proposed database for enquiries/content only if the owner approves a backend.

```text
Browser
  index.html
    main.js
    contentLoader.js -> local JSON fallback / public page API
    scrollCanvas.js -> preloaded frame bitmaps -> canvas
    overlays.js -> HTML overlays + GSAP timeline
    smoothScroll.js -> optional Lenis <-> ScrollTrigger
    contactForm.js -> POST /api/enquiries
    analytics.js -> POST /api/events
  admin.html (later, separate entry and noindex)
        |
        +---- Vercel CDN: HTML, CSS, JS, frames, posters, gallery
        +---- Vercel Functions: page config, enquiries, analytics, admin APIs
                                   |
                                Supabase Postgres/Auth/Storage (if approved)
```

## 2. Proposed stack
| Concern | Choice from supplied plan | Usage note |
|---|---|---|
| Build tooling | Vite | Multi-page build only if `/admin` ships |
| Language | Vanilla JavaScript ES modules | Keep public bundle small |
| Scroll animation | GSAP + ScrollTrigger | Scrub frame index using normalized progress |
| Smooth scroll | Lenis, optional | Integrate with ScrollTrigger; provide native-scroll fallback |
| Frame rendering | Canvas 2D + `ImageBitmap` | Use cover-fit and capped device-pixel ratio |
| Image delivery | AVIF with WebP fallback | Audit current files first; do not assume AVIF is already present |
| Hosting | Vercel | Static CDN plus serverless endpoints |
| Database | Supabase Postgres, optional for v1 | Use for enquiry records/content only if required |
| Authentication | Supabase Auth | Editor/admin only; no visitor accounts |
| Testing | Vitest + Playwright + Lighthouse CI | Unit, contract, browser and performance tests |

## 3. Application modules
| Module | Responsibility |
|---|---|
| `contentLoader.js` | Read home page content; API first only if implemented, then local JSON fallback |
| `frameManifest.js` | Validate frame count, paths, dimensions and format |
| `scrollCanvas.js` | Preload, decode, resize and draw current frame |
| `overlays.js` | Render accessible headline/body/CTA elements based on scroll ranges |
| `smoothScroll.js` | Optional Lenis lifecycle and ScrollTrigger refresh |
| `contactForm.js` | Client validation and submission states |
| `analytics.js` | Consent-aware, privacy-minimal event collection |
| `navigation.js` | Mobile menu, anchor scroll and active states |

## 4. Content contract
A page configuration should be data-driven so copy timing can change without altering renderer code.

```json
{
  "slug": "home",
  "title": "Modern Guideways",
  "metaDescription": "CNC, VMC and HMC telescopic cover service and manufacturing.",
  "frameSet": {
    "version": "v1",
    "desktopPath": "/frames/v1/desktop/",
    "mobilePath": "/frames/v1/mobile/",
    "desktopCount": null,
    "mobileCount": null,
    "filenamePattern": "frame_{index}.webp"
  },
  "overlays": [
    {"id":"hero","start":0.00,"end":0.16,"heading":"Precision Protection for CNC, VMC & HMC Machines","body":"Telescopic cover service and manufacturing by Modern Guideways."},
    {"id":"range","start":0.16,"end":0.48,"heading":"Machine Protection Components","body":"Telescopic covers, roller covers, apron covers, bellows, guide wipers and enclosing guards."}
  ]
}
```

The `null` counts are deliberate: actual counts must come from the frame audit. Do not commit the example as production data until replaced with measured values.

## 5. Rendering algorithm
1. Read viewport and `prefers-reduced-motion` before starting frame downloads.
2. If reduced motion is enabled, show poster and complete HTML content; skip sequence preload.
3. Select the best available frame set for the viewport. If no mobile set exists, use the approved fallback and test its crop.
4. Load keyframes first (for example every fourth frame), then intermediate frames, then remaining frames.
5. Map `progress` from 0 to 1 to `round(progress * (frameCount - 1))`.
6. Draw the nearest decoded frame. Keep the last successfully drawn frame during transient decode gaps.
7. Cap Canvas DPR at 2 and use `ResizeObserver`/viewport resize handling.
8. Dispose of decoded bitmaps and event listeners when the page is left or reinitialized.

## 6. Initial performance budgets (provisional)
| Metric | Initial target | Gate |
|---|---:|---|
| LCP | < 2.5 s on representative 4G test | Measure and tune |
| CLS | < 0.1 | Required |
| Desktop frame payload | ≤ 12 MB per selected format | Provisional until audit |
| Mobile frame payload | ≤ 5 MB per selected format | Provisional until audit |
| Interactive scrub readiness | ≤ 4 s on representative 4G | Required after baseline |
| Public JS (gzipped) | ≤ 70 KB excluding vendor only if separately budgeted | Track bundle |
| Animation frame time | Aim for < 16.7 ms at 60 Hz | Profile real devices |

These are engineering targets, not measured performance claims.

## 7. API summary (only if backend is approved)
| Method | Endpoint | Access | Purpose |
|---|---|---|---|
| GET | `/api/page/:slug` | Public | Published page config |
| POST | `/api/enquiries` | Public, validated/rate-limited | Store/route a quote enquiry |
| POST | `/api/events` | Public, rate-limited | Batch analytics events |
| GET | `/api/admin/me` | Editor/admin | Verify authenticated role |
| GET/PUT | `/api/admin/content/:id` | Editor/admin | Read/update draft content |
| POST | `/api/admin/content/:id/publish` | Editor/admin | Publish approved content |
| GET/DELETE | `/api/admin/enquiries/:id` | Admin only | View or delete enquiry record |

## 8. Security and reliability
- Keep the database service-role key on the server only.
- Validate and size-limit every public POST body.
- Add a honeypot and rate limit to enquiry submissions; add CAPTCHA only if spam justifies it.
- Apply deny-by-default database policies.
- Do not expose private enquiries in a public endpoint.
- Use static JSON content if the content API is unavailable.
- The animation must fail open to poster + text, not to a blank screen.
- Confirm current provider terms and limits before production deployment.

## 9. Decisions still required
- Confirm whether Supabase is needed for v1 or whether enquiries can use a simpler approved route.
- Confirm the exact frame count, file sizes, image dimensions and desktop/mobile availability.
- Confirm whether the video is one continuous shot or contains scene cuts.
- Confirm analytics consent policy and data retention with the company.
- Confirm desired editor workflow before building `/admin`.
