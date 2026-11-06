# 15 — Launch & Go-Live
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Launch roles
| Role | Responsibility |
|---|---|
| Business owner | Final claims, product photos, contact details and Go/No-Go |
| Project lead | Scope, schedule, approvals and communication |
| Frontend developer | Animation, responsive UI and content integration |
| Backend/DevOps | DNS, deployment, forms, database and monitoring |
| QA | Cross-device, accessibility, form and content validation |

One person may hold multiple roles, but each responsibility must have a named owner.

## 2. Relative launch schedule
| Timing | Activity |
|---|---|
| L-14 to L-10 | Verify content, image rights, domain ownership and enquiry routing |
| L-9 to L-7 | Release candidate on preview URL; complete frame audit and device tests |
| L-6 to L-4 | Fix P0/P1 issues; run accessibility/performance checks |
| L-3 | Business owner Go/No-Go review |
| L-2 | Confirm DNS records and production environment |
| L-1 | Deploy production build while retaining rollback path |
| L | DNS/canonical cutover, smoke tests and announcement |
| L+1 to L+3 | Close monitoring of errors, enquiries and performance |
| L+7 | First review and backlog |

Dates remain relative until kickoff and account readiness are known.

## 3. Go/No-Go checklist
- [ ] Owner approved every public statement and contact detail.
- [ ] Existing frame sequence passes audit and contact-sheet review.
- [ ] Desktop/mobile/reduced-motion experience works.
- [ ] All products shown are correctly identified.
- [ ] Form submission reaches the approved recipient/storage.
- [ ] Phone and email links tested.
- [ ] Privacy notice and terms reviewed if applicable.
- [ ] Domain and HTTPS verified.
- [ ] Sitemap, robots, canonical tags and metadata checked.
- [ ] No open P0/P1 defects.
- [ ] Monitoring and rollback owner named.

## 4. Launch-day smoke test
Open home, about, products, capabilities, gallery and contact routes. Scroll forward/backward through the animation. Test the first, middle and final frames. Switch reduced motion on. Submit one clearly labelled test enquiry and verify receipt. Click each phone/email link. Inspect mobile viewport and 404 page. Confirm final URL, HTTPS and social preview.

## 5. Rollback triggers
Rollback or disable the animation if the page is blank, key content is inaccessible, contact enquiries are lost, the wrong business/contact data is published, or severe mobile performance/accessibility defects appear. The static content page should remain available even if the scroll canvas is disabled.

## 6. Post-launch watch
Check error logs, frame 404s, form failures, notification delivery, uptime, and actual visitor behavior. Record issues and owner. Do not claim a conversion or performance target has been met until sufficient data is collected.

## 7. Handover
Provide repository access, deployment ownership, domain registrar access, environment-variable owner, content editing instructions, frame manifest, approved media folder, privacy/retention owner and next review date.
