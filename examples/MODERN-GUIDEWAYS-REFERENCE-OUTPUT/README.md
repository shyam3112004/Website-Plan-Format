# Modern Guideways — Scroll-Driven Website Build Guide
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## Project summary
A modern B2B manufacturing website for **Modern Guideways**, led by an immersive scroll-controlled image sequence. The project owner has already generated the video and split it into sequential image frames. The supplied screenshot shows files named `frame_0001.webp`, `frame_0002.webp` and onward inside a `frames/` folder. The implementation begins by auditing and integrating those existing files.

**Primary business line:** CNC & VMC & HMC telescopic cover service and manufacturing.

**Primary website goal:** convert industrial buyers and machine owners into quote/service enquiries.

## Company profile facts
## Verified source facts and publishing controls

The supplied Modern Guideways profile and business-card artwork identify the business as **Modern Guideways** and describe its activity as **CNC & VMC & HMC telescopic cover service and manufacturing**. The listed product/service categories are telescopic covers, roller covers, apron covers, bellows, guide wipers, enclosing guards and other CNC parts/design work. The profile describes custom designs, about 20 years of experience in telescopic-cover design, workmanship and raw-material quality, and the goal of making machinery easier to maintain and longer-lived. It also states the following vision and mission themes: precision, reliability, continuous innovation, quality, safety, on-time delivery, customer satisfaction and capability development.

| Field | Value shown in supplied materials | Publishing check |
|---|---|---|
| Business name | Modern Guideways | Use this exact spelling consistently |
| Managing Director | S. Arockiasamy | Confirm title/name presentation before publication |
| Main capability line | CNC & VMC & HMC telescopic cover service & manufacturing | Confirm preferred capitalization and wording |
| Phone | +91 80564 38763; +91 87781 21768 | Test both numbers and confirm which is primary |
| Email | modernguideways@gmail.com | Confirm inbox is monitored |
| Website printed on profile | www.modernguideways.com | Confirm domain ownership, DNS access and canonical host |
| Address | 2/186, Gollapatti (Vill), Selliam Patti (Po), Palacode (Tk), Dharmapuri (Dt), Tamil Nadu - 636 809 | Confirm postal spelling and PIN before launch |
| GSTIN printed on profile | 33FPHPR0275D1ZZ | Optional on public site; verify against official business records before displaying |
| Experience claim | 20 years in telescopic-cover design | Confirm start year and exact wording before publishing as a dated claim |

Do not invent customer names, certifications, machine brands, production capacity, delivery guarantees, tolerances, material grades, export markets, awards, warranty terms or engineering specifications. Add those only after the company provides approved evidence.

## Product scope
- Telescopic Covers
- Roller Covers
- Apron Covers
- Bellows
- Guide Wipers
- Enclosing Guards
- Other CNC parts/design work only where confirmed by the company

The website is enquiry-led. Product pricing, online checkout, subscriptions and payments are not part of v1.

## Recommended experience
The home page opens with a pinned Canvas stage. Scrolling maps to the existing image frames; HTML overlays provide approved headlines and explanatory text. Below the animation, normal page sections explain the company, products, capabilities, gallery, vision/mission and contact route. A static poster and stacked copy provide the no-JavaScript and reduced-motion experience.

## Proposed stack (from the supplied planning format)
- Vite + Vanilla JavaScript modules
- GSAP + ScrollTrigger for scroll-driven animation
- Lenis only if it passes native-scroll/accessibility testing
- Canvas 2D for image sequence rendering
- Vercel for hosting/serverless endpoints
- Supabase Postgres/Auth only if the business approves enquiry/content storage and a later admin workflow

## Quick start (after repository creation)
```bash
npm install
npm run dev
npm run lint
npm test
npm run build
npm run preview
```

Use `vercel dev` for local API-function testing if Vercel Functions are implemented. Keep `.env.local` out of Git.

## Recommended repository layout
```text
modern-guideways-site/
  README.md
  CHANGELOG.md
  index.html
  about/index.html
  products/index.html
  capabilities/index.html
  gallery/index.html
  contact/index.html
  privacy/index.html
  terms/index.html
  public/
    brand/
    frames/v1/desktop/
    frames/v1/mobile/
    frames/v1/manifest.json
    posters/hero.webp
    posters/og.jpg
    products/
    gallery/
    icons/
  src/
    main.js
    content/
    animation/
    components/
    services/
    styles/
    admin/                 # later release only
  api/                     # only when backend is approved
  supabase/migrations/     # only when Supabase is approved
  scripts/
    audit-frames.mjs
    make-contact-sheet.py
    export-content.mjs
  tests/
    unit/
    contract/
    e2e/
  docs/                    # 35 documents in this pack
```

## 9 work stages
1. **Scope and verify:** approve facts, logo, product names, contact routing and domain access.
2. **Storyboard and design:** inspect current frame contact sheet; map story overlays; design desktop/mobile/reduced-motion UI.
3. **Audit existing frames:** count, numeric order, gaps, dimensions, size and decode; optimize and version without re-extracting video.
4. **Set up frontend:** Vite scaffold, semantic HTML, tokens and content config.
5. **Build animation:** preloader, Canvas renderer, ScrollTrigger mapping, overlays, fallback and responsive behavior.
6. **Build company pages:** About, Products, Capabilities, Gallery, Contact and legal pages.
7. **Connect enquiry workflow:** select approved notification/storage path, validate form and protect personal data.
8. **Harden and launch:** accessibility, mobile, performance, SEO, security, domain, smoke tests and rollback.
9. **Operate:** monitor enquiries, update content/media, maintain accounts/backups and review performance.

## Build gates
| Gate | Must be true before moving forward |
|---|---|
| G1 Business approval | Name, phone, email, address, claims, logo and product categories confirmed |
| G2 Frame approval | Manifest complete, contact sheet reviewed, overlay mapping approved |
| G3 Design approval | Desktop/mobile/reduced-motion screens signed off |
| G4 Prototype approval | Scroll forward/backward works on preview and no blank frame gaps |
| G5 Enquiry approval | Test enquiry arrives through the chosen route and owner confirms receipt |
| G6 Launch approval | QA, privacy/legal, SEO, DNS, HTTPS, rollback and owner sign-off complete |

## Existing frame integration — important
Do **not** re-generate or re-split the video as a default task. First copy the current `frames/` folder to a versioned working directory. Audit its exact total count and numbering. Generate a contact sheet. If a mobile sequence is needed, create optimized mobile derivatives from the existing frames after crop review. Keep the originals untouched. See [07-ASSET-PIPELINE.md](docs/07-ASSET-PIPELINE.md).

## Hermes Agent Skills

Use [SKILLS-AND-AGENT-MAP.md](SKILLS-AND-AGENT-MAP.md) as the separate companion guide for choosing, inspecting, installing and invoking Hermes skills for each of the 35 documents. It includes the per-document skill matrix and distinguishes minimum-launch work from deferred backend/payment templates.

## Document index
The complete document list is in [MASTER-DOCUMENTATION.md](MASTER-DOCUMENTATION.md). The 35 documents are grouped below:

### Product, brand and content
- [01 PRD](docs/01-PRD.md)
- [03 App Flow](docs/03-APP-FLOW.md)
- [04 UI/UX Design Brief](docs/04-UI-UX-DESIGN-BRIEF.md)
- [10 Content Editor Guide](docs/10-CONTENT-EDITOR-GUIDE.md)
- [35 User Guide / FAQ](docs/35-USER-GUIDE-FAQ-TROUBLESHOOTING.md)

### Frontend, animation and assets
- [02 TRD](docs/02-TRD.md)
- [07 Asset Pipeline](docs/07-ASSET-PIPELINE.md)
- [18 Frontend Architecture](docs/18-FRONTEND-ARCHITECTURE.md)
- [20 Component Library](docs/20-COMPONENT-LIBRARY-DESIGN-SYSTEM.md)
- [21 State Management](docs/21-STATE-MANAGEMENT.md)
- [24 File & Media Management](docs/24-FILE-MEDIA-MANAGEMENT.md)
- [26 Accessibility](docs/26-ACCESSIBILITY-STANDARD.md)
- [27 Performance](docs/27-PERFORMANCE-ENGINEERING.md)
- [33 Browser Compatibility](docs/33-BROWSER-DEVICE-COMPATIBILITY.md)

### Backend, integrations and security
- [05 Backend Schema](docs/05-BACKEND-SCHEMA.md)
- [06 Authentication](docs/06-AUTHENTICATION.md)
- [19 Backend Architecture](docs/19-BACKEND-ARCHITECTURE.md)
- [22 API Documentation](docs/22-API-DOCUMENTATION.md)
- [23 Integration Documentation](docs/23-INTEGRATION-DOCUMENTATION.md)
- [25 Payment Scope](docs/25-PAYMENT-SUBSCRIPTION-DOCUMENTATION.md)
- [28 Disaster Recovery](docs/28-DISASTER-RECOVERY-BACKUP-PLAN.md)
- [29 Logging & Monitoring](docs/29-LOGGING-MONITORING-OBSERVABILITY.md)
- [32 Roles & Permissions](docs/32-USER-ROLE-PERMISSION-MANAGEMENT.md)
- [34 Data Migration](docs/34-DATA-MIGRATION-PLAN.md)

### Launch, business operations and governance
- [08 Deployment Runbook](docs/08-DEPLOYMENT-RUNBOOK.md)
- [09 QA Checklist](docs/09-QA-CHECKLIST.md)
- [11 Privacy & Legal](docs/11-PRIVACY-AND-LEGAL.md)
- [12 Analytics](docs/12-ANALYTICS-SPEC.md)
- [13 Domain, DNS & Email](docs/13-DOMAIN-DNS-EMAIL.md)
- [14 SEO](docs/14-SEO-AND-DISCOVERABILITY.md)
- [15 Launch & Go-Live](docs/15-LAUNCH-AND-GO-LIVE.md)
- [16 Maintenance & Operations](docs/16-MAINTENANCE-AND-OPERATIONS.md)
- [30 Git Workflow](docs/30-GIT-VERSION-CONTROL-WORKFLOW.md)
- [31 Environment Configuration](docs/31-ENVIRONMENT-CONFIGURATION.md)
- [17 Implementation Plan — final reading-order document](docs/17-IMPLEMENTATION-PLAN.md)

## Open decisions before implementation
1. Confirm the current frame folder and transfer the full folder into the project repository.
2. Confirm whether the site should use one continuous animation or split the sequence into chapters.
3. Confirm primary enquiry destination: email only, Supabase storage, or both.
4. Confirm company claims, exact address spelling, phone priority and whether GSTIN should appear publicly.
5. Confirm domain registrar access and canonical URL.
6. Provide clean logo files and approved product/workshop images.

## Source integrity
This plan is tailored from the uploaded `Website Plan Format.md`, Modern Guideways profile PDF (pages 1–2), the business-card image and the screenshot showing the existing frame folder. Exact frame metrics, product specifications, certifications, company claims beyond the supplied profile, DNS status and enquiry workflow remain open until verified.
