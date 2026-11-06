# 20 — Component Library & Design System
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Design principles
Clarity, precision, restrained motion, real product evidence, mobile usability and consistent contact actions.

## 2. Tokens
Use the proposal in `04-UI-UX-DESIGN-BRIEF.md`. Store tokens as CSS custom properties. Typography, spacing, radius, shadow, container widths, z-index and motion durations should be centralized.

## 3. Component inventory
| Component | Variants | Required states |
|---|---|---|
| Header | desktop/mobile | default, menu-open, scrolled |
| Button | primary, secondary, text | hover, focus, active, disabled |
| Scroll loader | full, compact | loading, ready, timeout, error |
| Overlay | left, center, right | hidden, entering, visible, exiting |
| Product card | image, compact | default, hover, focus |
| Gallery item | grid/lightbox | default, focused, expanded |
| Contact card | phone/email/location | hover, focus |
| Enquiry form | full-width/compact | pristine, invalid, submitting, success, error |
| Footer | full | default, mobile stacked |

## 4. Component rules
Use semantic HTML and CSS first. Use buttons for actions and anchors for navigation. Avoid clickable `div` elements. Provide alt text, labels, focus styles and touch target sizing. Avoid duplicate styles in page-specific selectors.

## 5. Motion governance
Use a shared duration/easing scale. The scroll canvas is the primary motion feature; all other motion is subordinate. Every animated component has a reduced-motion behavior.

## 6. Governance
A component change must update its usage notes and visual tests. New tokens require design-owner approval. Keep the MG logo clear space and aspect ratio; do not recolor or redraw without approval.
