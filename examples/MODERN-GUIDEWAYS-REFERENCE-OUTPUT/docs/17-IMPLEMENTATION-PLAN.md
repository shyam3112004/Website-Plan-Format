# 35 — Implementation Plan
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Plan assumptions
This is a relative schedule, not a dated commitment. The company website is a B2B enquiry site, not an online store. The scroll video and extracted frame sequence already exist; work starts with auditing the current frames. Schedule depends on asset audit, owner feedback, approved product photography, domain access and the chosen enquiry-storage workflow.

## 2. Delivery stages
| Stage | Workstream | Main documents | Exit gate |
|---|---|---|---|
| 1 | Scope and business facts | 01, 10, 11, 13 | Owner approves scope, claims and contact details |
| 2 | Storyboard and UX | 03, 04, 20 | Contact sheet reviewed; site flow and design approved |
| 3 | Existing frame audit | 07, 24, 27 | Manifest, contact sheet and payload report pass |
| 4 | Technical foundation | 02, 18, 19, 21, 31 | Local app structure and fallback path work |
| 5 | Frontend implementation | 18, 20, 21, 26, 33 | Scroll stage, content sections and responsive layout work |
| 6 | Backend and enquiry flow | 05, 06, 19, 22, 23, 32 | Enquiry reaches approved destination securely |
| 7 | SEO, privacy and performance | 11, 12, 14, 26, 27, 29 | Required review and budget checks pass |
| 8 | QA and launch | 08, 09, 13, 15 | Go/No-Go checklist signed |
| 9 | Operations and future admin | 16, 28, 30, 34, 35 | Handover, restore and ownership complete |

## 3. Suggested week-by-week sequence
| Period | Tasks | Deliverables |
|---|---|---|
| Week 1 | Confirm company facts, product scope, domain, logo master, contact route; inventory current frames and create contact sheet | Approved content sheet; frame audit report |
| Week 2 | Finalize sitemap, storyboard overlays, design tokens and responsive screen layouts | Approved design and scroll storyboard |
| Week 3 | Scaffold Vite app, implement sticky stage, Canvas renderer, frame loader and static fallback | Working animation prototype |
| Week 4 | Build product/about/capabilities/gallery/contact sections; wire forms and analytics if approved | Full site feature preview |
| Week 5 | Optimize frames, mobile behavior, accessibility, SEO, privacy pages and error states | Release candidate |
| Week 6 | Cross-device QA, owner corrections, DNS cutover, smoke tests and launch | Live v1 + handover |

These periods are planning estimates only; adjust after the frame audit and team capacity review. A custom admin portal can be a later release.

## 4. Task ownership
| Task | Responsible role | Approver |
|---|---|---|
| Business facts and claims | Business owner/editor | Managing Director or delegate |
| Product descriptions/specs | Product/engineering owner | Business owner |
| Frame audit and storyboard | Designer/frontend developer | Business owner |
| UI and implementation | Frontend developer | Project lead |
| Enquiry/backend/security | Backend/DevOps | Business owner + technical lead |
| Privacy and legal text | Business/legal adviser | Business owner |
| QA and launch | QA/DevOps | Business owner |

## 5. Detailed task checklist
### Stage 1 — Foundation
- [ ] Confirm the public name, logo master and approved brand palette.
- [ ] Verify phone, email, address, GSTIN display decision and domain access.
- [ ] Confirm whether the 20-year experience claim may be published and how it should be phrased.
- [ ] Approve MVP pages and non-goals.
- [ ] Decide whether enquiry form submissions are email-only, database-backed or both.

### Stage 2 — Design and storyboard
- [ ] Generate a contact sheet of the existing frames.
- [ ] Identify the visual subject at the beginning, middle, end and scene transitions.
- [ ] Map overlay copy to progress windows.
- [ ] Mark text-safe zones and mobile crop issues.
- [ ] Approve desktop, tablet, mobile and reduced-motion screens.

### Stage 3 — Asset readiness
- [ ] Numerically sort and audit files named `frame_0001.webp` style.
- [ ] Confirm complete sequence, dimensions, payload and image decode.
- [ ] Create or derive mobile frame variants if required.
- [ ] Export poster and social preview image.
- [ ] Create versioned paths and manifest.

### Stage 4/5 — Frontend
- [ ] Create Vite project, tokens and semantic page structure.
- [ ] Implement Canvas cover-fit renderer and progressive loading.
- [ ] Integrate GSAP ScrollTrigger; add Lenis only after native-scroll behavior passes.
- [ ] Build overlay timeline from config.
- [ ] Build all content sections and navigation.
- [ ] Implement form states, call/email links and reduced-motion mode.

### Stage 6 — Backend
- [ ] Choose enquiry route and data retention policy.
- [ ] Implement server validation, rate limits and anti-spam controls.
- [ ] Enforce database RLS if Supabase is used.
- [ ] Verify notification delivery and failure handling.
- [ ] Defer admin portal until editor requirements are confirmed.

### Stage 7/8 — Harden and launch
- [ ] Validate metadata, sitemap, canonical host and legal pages.
- [ ] Run accessibility and real-device checks.
- [ ] Profile scroll performance on representative mobile hardware.
- [ ] Verify contact data and one real test enquiry.
- [ ] Sign owner approval and launch.

## 6. Milestones
| Milestone | Evidence |
|---|---|
| M1 — Scope locked | Signed PRD and approved business-fact sheet |
| M2 — Assets approved | Frame audit, contact sheet and storyboard sign-off |
| M3 — Animation demo | Preview URL with reversible frame scrubbing |
| M4 — Content complete | All page sections, products and contact paths |
| M5 — Release candidate | QA, accessibility, SEO and performance evidence |
| M6 — Production launch | Domain, HTTPS, form delivery and smoke tests pass |
| M7 — Handover | Access register, operating guide, backups and owners |

## 7. Risks and mitigations
| Risk | Mitigation |
|---|---|
| Current frames not suitable for mobile | Create responsive derivatives from existing images or use poster/static layout |
| Video doesn't visually match product copy | Use neutral overlays and genuine product photographs; do not mislabel frames |
| Domain/registrar access delayed | Verify at kickoff and build on preview domain meanwhile |
| Contact form destination undecided | Do not ship form until a monitored recipient/storage route is approved |
| Product facts incomplete | Publish category names only; add specs after written approval |
| Scope expands into ecommerce/admin | Keep payments and portal out of MVP; require formal scope change |

## 8. Definition of done
- [ ] Every route is responsive and navigable.
- [ ] Scroll sequence passes numeric frame audit and storyboard review.
- [ ] Static/reduced-motion content is complete.
- [ ] Product copy and media are owner-approved.
- [ ] Enquiry delivery works and privacy handling is approved.
- [ ] Accessibility, SEO, performance and security checks are complete.
- [ ] Deployment and rollback are tested.
- [ ] Docs and content editor handover are complete.

## 9. Document coverage map
| Document | Main implementation stage |
|---|---|
| README / Build Guide | All stages |
| 01 PRD | Stage 1, acceptance in Stage 8 |
| 02 TRD | Stages 4–6 |
| 03 App Flow | Stages 2 and 5 |
| 04 UI/UX Design Brief | Stage 2, implementation in 5 |
| 05 Backend Schema | Stage 6 |
| 06 Authentication | Stage 6 or later admin release |
| 07 Asset Pipeline | Stages 1–3 and future frame releases |
| 08 Deployment Runbook | Stages 4 and 8–9 |
| 09 QA Checklist | Stages 5–8 |
| 10 Content Editor Guide | Stages 1 and 9 |
| 11 Privacy & Legal | Stages 1, 6 and 8 |
| 12 Analytics | Stage 7 |
| 13 Domain, DNS & Email | Stages 1 and 8 |
| 14 SEO | Stage 7 |
| 15 Launch & Go-Live | Stage 8 |
| 16 Maintenance & Operations | Stage 9 |
| 18 Frontend Architecture | Stages 4–5 |
| 19 Backend Architecture | Stage 6 |
| 20 Component Library | Stages 2 and 5 |
| 21 State Management | Stage 5 |
| 22 API Documentation | Stage 6 |
| 23 Integration Documentation | Stage 6 |
| 24 File & Media Management | Stages 2–3 |
| 25 Payment & Subscription | Explicitly out of scope; revisit only by change request |
| 26 Accessibility | Stages 2, 5 and 8 |
| 27 Performance Engineering | Stages 3, 5 and 8 |
| 28 Disaster Recovery | Stage 9 |
| 29 Observability | Stages 6–9 |
| 30 Git Workflow | All engineering stages |
| 31 Environment Configuration | Stage 4 |
| 32 Roles & Permissions | Stage 6 or later admin release |
| 33 Browser & Device Compatibility | Stages 5 and 8 |
| 34 Data Migration | Stage 6 if DB is adopted |
| 35 User Guide & FAQ | Stage 9 |

## 10. Decisions log
| Decision | Current proposal | Owner | Status |
|---|---|---|---|
| Canonical domain | `www.modernguideways.com` pending ownership/DNS check | Business owner | Open |
| Enquiry storage | Server-side email and/or Supabase after approval | Business owner | Open |
| Public admin portal | Defer beyond v1 | Business owner | Proposed |
| Mobile frame set | Audit and derive from existing frames if required | Frontend | Open |
| Experience claim | “20 years” only after owner confirmation | Business owner | Open |
| Payments | Excluded from MVP | Business owner | Proposed |
