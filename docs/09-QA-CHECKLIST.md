<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 09 — Qa Checklist

# QA Checklist
**Related:** [PRD](./01-PRD.md) §8, §12 · [TRD](./02-TRD.md) §5, §11 · [App Flow](./03-APP-FLOW.md) · [Authentication](./06-AUTHENTICATION.md) §7 · [Implementation Plan](./17-IMPLEMENTATION-PLAN.md) §3

## 1. How to Use This
Run the full list on the release-candidate preview URL. Record each result as Pass, Fail (with a ticket) or N/A. A release can ship only when no P0 or P1 checks fail.

| Severity | Meaning | Example |
|---|---|---|
| P0 | Core experience or data safety broken | Scrub does not work on iOS; leads readable by anyone |
| P1 | Major issue with a workaround | Overlay timing wrong; error message missing |
| P2 | Minor | Small spacing issue |

## 2. Device Matrix
| Device | Browser | Priority |
|---|---|---|
| MacBook (Retina) | Safari, Chrome | P0 |
| Windows laptop | Chrome, Edge, Firefox | P0 |
| iPhone 13+ | Safari | P0 |
| Pixel 7 / Galaxy A54 | Chrome | P0 |
| iPad | Safari | P1 |
| Low-end Android (4 GB RAM) | Chrome | P1 |

## 3. Functional Checks
| ID | Check | Expected |
|---|---|---|
| F-01 | Open `/`, scroll through the scroll section | Frames change 1:1 with scroll |
| F-02 | Scroll back up | Frames reverse smoothly, no skipped images |
| F-03 | Watch the loader on a cold load | Progress rises to 100%, scroll is locked until ready |
| F-04 | Compare overlays with the storyboard at 0, 25, 50, 75, 100% | Text fades in and out at the configured ranges |
| F-05 | Reach the end of the scroll section | Canvas scales to 0.85 with rounded corners |
| F-06 | Click the overlay CTA | Page scrolls to `#signup` |
| F-07 | Resize the window mid-scroll | Canvas redraws, no blank frame, no layout break |
| F-08 | Rotate a phone mid-scroll | Same as F-07; the frame set does not switch |
| F-09 | Turn on reduced motion in the OS | Poster image and stacked text, no scrub, no animation beyond 150 ms fades |
| F-10 | Disable JavaScript | `<noscript>` poster and all text visible |
| F-11 | Use anchors and the footer links | Smooth scroll; `/privacy` and `/terms` load |

## 4. Lead Form
| ID | Check | Expected |
|---|---|---|
| L-01 | Submit a valid email with consent | Success state, one row in `leads` |
| L-02 | Submit an invalid email | Inline error linked with `aria-describedby`, values kept |
| L-03 | Submit without ticking consent | Inline error, nothing sent |
| L-04 | Submit the same email twice | Success message both times, one row only |
| L-05 | Fill the hidden `website` field (via dev tools) | Looks successful, no row saved |
| L-06 | Submit 6 times within a minute | The sixth is rejected with a rate-limit message |
| L-07 | Block `/api/leads` (offline) | Inline error, form values kept |
| L-08 | Check the consent text and link | Matches [Privacy & Legal §3](./11-PRIVACY-AND-LEGAL.md); the checkbox starts unchecked |

## 5. Failure and Network Scenarios
| ID | Scenario | Expected |
|---|---|---|
| N-01 | Throttle to slow 3G | Loader stays honest; "Continue anyway" after 10 s |
| N-02 | Click "Continue anyway" with partial frames | Scroll unlocks; scrub uses the nearest loaded frame |
| N-03 | Block `/api/page/home` | Page loads from `/content/home.json` |
| N-04 | Make one frame return 404 | No error shown; the nearest loaded frame is drawn |
| N-05 | Block `/api/events` | No visible effect |
| N-06 | Go offline after load | Page remains usable; form shows an error |

## 6. Performance
| ID | Check | Target |
|---|---|---|
| P-01 | Lighthouse Performance | ≥ 85 |
| P-02 | LCP | < 2.5 s |
| P-03 | CLS | < 0.05 |
| P-04 | Public JS / CSS (gzipped) | ≤ 60 KB / ≤ 10 KB |
| P-05 | Frames total | ≤ 12 MB desktop, ≤ 5 MB mobile |
| P-06 | Time to interactive scrub on 4G | ≤ 3 s desktop, ≤ 4 s mobile |
| P-07 | Scrub frame rate on a 2022 mid-range Android | ≥ 55 fps |
| P-08 | Main-thread work per frame | ≤ 8 ms desktop, ≤ 12 ms mobile |
| P-09 | `/admin` code in the public bundle | None (`supabase-js` absent from public JS) |

## 7. Accessibility (WCAG 2.2 AA)
| ID | Check | Expected |
|---|---|---|
| A-01 | Lighthouse Accessibility and an axe scan | ≥ 95, no critical issues |
| A-02 | Keyboard only: tab through the page | Logical order, visible 2 px focus ring |
| A-03 | Screen reader (VoiceOver, NVDA) | Overlay text is read in order; form labels and errors announced |
| A-04 | Text over video | Contrast ≥ 4.5:1 with the scrim |
| A-05 | Touch targets | ≥ 44×44 px |
| A-06 | Zoom to 200% | No loss of content |
| A-07 | Admin screens (v1.1) | Keyboard-only sign-in and table use; focus returns after dialogs close |

## 8. SEO, Domain and Email
| ID | Check | Expected |
|---|---|---|
| S-01 | Lighthouse SEO | ≥ 95 |
| S-02 | Page source | Real HTML text for overlays and features, title, meta description, OG tags |
| S-03 | `robots.txt` and sitemap | Public pages listed; `/admin` absent |
| S-04 | Share the URL in a chat app | OG image and title render |
| S-05 | View source of the built page | Title, description, canonical, OG tags and overlay text are present without running JavaScript |
| S-06 | Open every URL in `sitemap.xml` | All return 200 on the canonical host |
| S-07 | Run the structured data in a validator | No errors |
| S-08 | Open an unknown URL | Custom 404 page with a 404 status |
| S-09 | Favicons and manifest | All icons load in a browser tab and on an iOS home screen |
| D-01 | Open the canonical `https://` URL | Valid certificate, 200 |
| D-02 | Open the other host (`www` or apex) and the `http://` version | Redirect to the canonical HTTPS URL |
| D-03 | Send a test email from each mailbox and the auth sender, then view the original message | SPF, DKIM and DMARC pass |
| D-04 | Request a sign-in link (v1.1) | Arrives in the inbox, not spam, from the domain sender |
| D-05 | Compare live DNS (`dig`) with the table in [Domain §3](./13-DOMAIN-DNS-EMAIL.md) | Match; exactly one SPF record |

## 9. Analytics
| ID | Check | Expected |
|---|---|---|
| E-01 | Load, scroll to the end, submit a lead, then query `analytics_events` for the session | `page_view`, `loader_complete`, four `scroll_milestone` rows, `lead_submit` (see [Analytics Spec](./12-ANALYTICS-SPEC.md)) |
| E-02 | Reload the page in the same tab | Same `session_id`; milestones fire again only once per page load |
| E-03 | Check payloads | No email, name or other personal data in `props` |

## 10. Security and Privacy
| ID | Check | Expected |
|---|---|---|
| X-01 | Query `leads` with the anon key | Denied |
| X-02 | `grep -r "service_role" dist/` | No matches |
| X-03 | Response headers on `/` | Public CSP, `nosniff`, referrer policy |
| X-04 | Response headers on `/frames/*` | `immutable` cache header |
| X-05 | Inspect a stored lead | IP stored only as a hash |
| X-06 | `/privacy` and `/terms` | Live, reviewed, consent text matches |

## 11. Admin and Authentication (v1.1)
| ID | Check | Expected |
|---|---|---|
| U-01 | Call every `/api/admin/*` route without a token | 401 on all |
| U-02 | Sign in as an editor, call a leads route | 403 |
| U-03 | Sign in as an admin without MFA, call a leads route | 403 `mfa_required` |
| U-04 | Disable an editor, then use their existing session | 403 on the next request |
| U-05 | Request a link for an unknown email | Same message as for a known email |
| U-06 | Try to disable the last active admin | Rejected |
| U-07 | Edit and publish a page as an editor | Change is live within the cache window; an `audit_log` row exists |
| U-08 | Check `/admin` response headers | `noindex`, `no-store`, admin CSP |
| U-09 | Delete a test lead as an admin | Row gone; `audit_log` row written |

## 12. Sign-Off
| Item | Result |
|---|---|
| Release / version | |
| Preview URL | |
| Date | |
| P0 failures | |
| P1 failures | |
| Lighthouse (Perf / A11y / SEO) | / / |
| Devices covered | |
| QA sign-off | Name: ______  Date: ______ |
| Product sign-off | Name: ______  Date: ______ |

---
