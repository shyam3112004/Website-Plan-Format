# 01 — Product Requirements Document (PRD)
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Product overview
Build a premium, responsive B2B website for Modern Guideways. The home page opens with an immersive scroll-controlled animation driven by the image frames that the project owner has already generated. The rest of the site explains the company, product categories, capabilities and enquiry path using real company text and photography.

## 2. Business problem
The company profile contains useful manufacturing information, but a web visitor needs to understand within seconds what Modern Guideways makes, which machine types it serves, how to request a quotation and how to contact the company. The website should turn the existing frame sequence into a clear product story without making essential information depend on motion.

## 3. Product goals and success measures
| Goal | Initial success measure | Measurement |
|---|---|---|
| Explain the business quickly | Visitors can identify the CNC/VMC/HMC cover focus from the first screen | Five-second comprehension test |
| Show the product range | All six named product categories are discoverable | Product-section QA |
| Generate qualified enquiries | Enquiry submissions and phone/email actions are trackable | Form and CTA events |
| Deliver a smooth scroll story | No visible frame gaps or long stalls on target devices | Real-device profiling |
| Support search discovery | Each core page has crawlable text and metadata | Search Console and SEO checks |
| Keep content maintainable | Business text and product descriptions can be updated without rewriting animation code | Editor walkthrough |

Targets such as conversion rate, Lighthouse scores and response times must be baselined after launch; do not treat unmeasured targets as achieved results.

## 4. Target audiences
- Procurement and purchase teams sourcing CNC machine protection components.
- CNC/VMC/HMC machine owners or maintenance teams looking for covers, wipers or related parts.
- OEMs, machine builders and industrial automation businesses evaluating a manufacturing partner.
- Existing customers seeking contact details or service enquiries.

## 5. Proposed site map
| Route | Purpose | MVP |
|---|---|---|
| `/` | Immersive home page, company intro, products, capabilities, proof gallery and enquiry CTA | Yes |
| `/about/` | Company profile, experience statement, vision and mission | Yes |
| `/products/` | Overview of all product categories; category detail sections or pages | Yes |
| `/capabilities/` | Design, manufacturing and service scope, using confirmed facts only | Yes |
| `/gallery/` | Approved product and workshop photography | Yes, when assets are supplied |
| `/contact/` | Address, phone, email and enquiry form | Yes |
| `/privacy/` | Privacy notice | Before collecting personal data |
| `/terms/` | Website terms / disclaimer | Legal review required |
| `/admin/` | Private editor area, invite-only | Later release; not public navigation |

## 6. Home page content sequence
1. Header with MG logo, navigation and Request a Quote action.
2. Scroll-animation hero with the proposed headline “Precision Protection for CNC, VMC & HMC Machines”.
3. A clear text summary of telescopic-cover service and manufacturing.
4. About Modern Guideways and the approved experience statement.
5. Product-category cards: telescopic covers, roller covers, apron covers, bellows, guide wipers and enclosing guards.
6. Manufacturing/service capability section.
7. Real product and workshop gallery.
8. Vision and mission.
9. Enquiry form and full contact information.
10. Footer with legal links and verified business details.

## 7. Functional requirements
| ID | Requirement | Priority |
|---|---|---|
| MG-FR-01 | Scroll position maps to the current frame in a sticky canvas stage | P0 |
| MG-FR-02 | Existing `frame_0001.webp`-style files are inventoried and loaded in numerical order | P0 |
| MG-FR-03 | Frame preloader shows progress and permits a safe continue fallback | P0 |
| MG-FR-04 | HTML text overlays are synchronized to normalized scroll progress | P0 |
| MG-FR-05 | Visitor can scroll both forward and backward without losing frame position | P0 |
| MG-FR-06 | Mobile layout has an appropriately sized frame set or a tested lightweight fallback | P0 |
| MG-FR-07 | Reduced-motion users receive a static poster and equivalent readable content | P0 |
| MG-FR-08 | Product categories and company details are crawlable HTML, not text baked into frames | P0 |
| MG-FR-09 | Phone links use `tel:` and email links use `mailto:` | P0 |
| MG-FR-10 | Enquiry form validates input and shows success/error states | P0 |
| MG-FR-11 | Enquiries are sent to an approved destination and stored only if the company approves storage | P0 |
| MG-FR-12 | Analytics measure section depth, product interest, CTA clicks and successful enquiries | P1 |
| MG-FR-13 | A static content fallback works if the content API is unavailable | P1 |
| MG-FR-14 | Invite-only editor/admin access is added only when the owner approves the admin scope | P2 |

## 8. Non-functional requirements
- Responsive from narrow mobile screens through large desktop layouts.
- Accessible keyboard navigation, visible focus, semantic headings, sufficient contrast and reduced-motion support.
- Animation must never block contact details or core text.
- No autoplay audio and no forced audio playback.
- Use compressed modern image formats with a safe fallback.
- No credentials, private keys or database service-role secrets in the browser bundle.
- Search engines must receive meaningful text, metadata and canonical URLs.

## 9. Out of scope for v1
- Online checkout, product pricing catalogue or payments.
- Visitor accounts, customer portal or order tracking.
- Live product configurator, 3D/WebGL simulation or engineering CAD download portal.
- Unverified certifications, technical specifications, client logos or testimonials.
- Multi-language publishing until translated copy is approved.

## 10. Assumptions and dependencies
- The animation video and extracted frames already exist; the delivered folder, exact count, dimensions, file sizes and complete sequence still need to be audited.
- The supplied profile is the content source of truth until the company approves updated copy.
- The logo artwork supplied in the profile is reference artwork; obtain a clean transparent SVG/PNG master before implementation.
- The profile lists `www.modernguideways.com`; domain access and live status must be confirmed.
- The proposed technical stack is inherited from the supplied website-plan format and must be validated at kickoff.

## 11. Risks
| Risk | Impact | Mitigation |
|---|---|---|
| Existing frames are missing or misnumbered | Scrub gaps or 404s | Generate a contact sheet and manifest before coding |
| Frame payload is too large | Slow mobile load | Compress, resize and create a mobile set from existing frames |
| Video sequence does not depict every planned section | Story becomes misleading | Use animation for the opening story; use real still photography for other sections |
| Unverified company claims are published | Trust/legal risk | Owner approval checklist for every claim |
| Contact form is not monitored | Lost enquiries | Confirm destination and response owner before launch |
| Animation harms accessibility/performance | Excludes visitors | Static text-first fallback and real-device QA |

## 12. Release acceptance
MVP is accepted when all P0 requirements pass, contact details are verified, enquiry routing is tested end to end, the complete frame set is audited, mobile and reduced-motion fallbacks work, and the owner signs off the rendered pages.
