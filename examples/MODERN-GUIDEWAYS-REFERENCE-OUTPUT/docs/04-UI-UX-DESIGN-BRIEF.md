# 04 — UI/UX Design Brief
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Design objective
Create a premium, engineered, precise visual experience that feels like an established manufacturing partner—not a generic technology landing page. The animation should attract attention, while typography, product details and contact information remain clear and practical.

## 2. Brand source and direction
The supplied MG logo/business-card artwork uses a dark navy/black wordmark with a blue-to-indigo gradient. The company profile uses navy, white and yellow geometric accents. Proposed direction: white/light surfaces, deep navy typography, cool blue/indigo motion accents, and restrained yellow highlights. Obtain official vector logo and a brand approval before finalizing tokens.

## 3. Proposed design tokens (starting point)
| Token | Proposed value | Use |
|---|---|---|
| `--navy-950` | `#071B3A` | Hero/footer, strongest headings |
| `--navy-800` | `#12366B` | Primary brand text |
| `--blue-600` | `#2457B5` | Primary CTA and links |
| `--indigo-500` | `#6574D8` | Gradient accent drawn from logo direction |
| `--yellow-400` | `#F2D33D` | Small accent, bullets and markers |
| `--ink` | `#162235` | Body text |
| `--muted` | `#64748B` | Secondary text |
| `--surface` | `#FFFFFF` | Main background |
| `--surface-alt` | `#F4F7FB` | Alternating sections |
| `--line` | `#D9E1EC` | Borders and separators |

Check all foreground/background pairs for contrast; these are not pre-certified accessible combinations.

## 4. Typography and layout
- Use a clear sans-serif family with strong numerals for engineering/industrial credibility.
- Headings: compact, bold, sentence case; avoid excessive all-caps paragraphs.
- Body copy: short lines and readable line height.
- Use a 12-column desktop grid, generous margins, and a single-column mobile layout.
- Use actual machine/product photography rather than decorative stock machinery.

## 5. Core components
- Header with MG logo, primary nav and Request a Quote button.
- Scroll progress/loader with accessible status text.
- Canvas stage with overlay container.
- Product category cards with image, title, short approved summary and enquiry link.
- Capabilities list with evidence-based copy.
- Gallery grid with lightbox only if keyboard accessible.
- Vision/mission panels.
- Contact cards for phone, email and address.
- Enquiry form with inline validation, loading, success and retry states.
- Footer with legal links and verified business details.

## 6. Motion specification
- Main canvas: scroll-scrubbed; reverse scroll must reverse the frame sequence.
- Text overlays: short fades/transforms; no rapid flashing, spinning or strobing.
- Section reveals: subtle and optional; do not animate every paragraph.
- Avoid parallax on small text and controls.
- Respect `prefers-reduced-motion`; use poster and static content.
- Lenis smooth scroll is optional; native browser scroll is the fallback.
- Do not delay access to contact details until the animation is complete.

## 7. Responsive screens
| Screen | Layout behavior |
|---|---|
| Desktop ≥ 1200 px | Full pinned hero; wide product grid; layered overlays in approved safe zones |
| Tablet 768–1199 px | Shorter sticky stage; two-column product grid; reduced overlay width |
| Mobile < 768 px | Single column; compact header; mobile frame set or poster/static story; CTA remains prominent |
| Reduced motion | Static hero image, stacked story text, no forced scroll sequence |

## 8. Content tone
Professional, specific, manufacturing-focused and concise. Prefer “CNC, VMC and HMC telescopic-cover service and manufacturing” over vague phrases such as “next-generation solutions”. Avoid unverified claims like “India's No. 1”, “world-class certified”, or quantified performance guarantees.

## 9. Design deliverables
- Sitemap and wireframes for all MVP routes.
- Desktop/tablet/mobile high-fidelity screens.
- Scroll-state designs at 0%, 25%, 50%, 75% and 100% of the actual animation.
- Frame/contact-sheet review with overlay safe areas.
- Product card and gallery component states.
- Form, loader, error, empty and reduced-motion states.
- Final token sheet and approved logo assets.
