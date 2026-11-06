<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 26 — ACCESSIBILITY STANDARD

**Suggested filename:** `26-ACCESSIBILITY-STANDARD.md`  
**Purpose:** Accessibility requirements and test procedures for public and admin experiences.

## 1. Standard and target
State the applicable WCAG version and conformance target approved for the project. Record any jurisdiction-specific obligations. Treat this as an engineering target, not a certification claim.

## 2. Requirements
| Area | Requirement | Verification |
|---|---|---|
| Keyboard | All interactive controls reachable and operable | Keyboard-only test |
| Focus | Visible focus and logical order | Manual + automated |
| Contrast | Text and controls meet selected contrast target | Contrast analyzer |
| Forms | Labels, instructions, errors and status announcements | Screen reader test |
| Motion | Respect `prefers-reduced-motion` | OS setting test |
| Canvas | Provide equivalent meaningful text and static poster | No-canvas / reduced-motion test |
| Images | Useful alternative text for informative images | Content review |

## 3. Scroll-driven experience
Document keyboard scrolling, focus behavior, loader announcements, canvas fallback, text availability without JavaScript, and how users can reach the CTA without relying on animation.

## 4. Admin accessibility
Specify accessible tables, dialogs, form validation, status messages, MFA flows, and session-expiry handling.

## 5. Testing
Record automated tools, manual screen-reader/browser combinations, test owners, defects, severity, and remediation dates.

## 6. Exceptions
Document each exception, user impact, mitigation, owner, approval, and expiry date. No exception is silently accepted.


---
