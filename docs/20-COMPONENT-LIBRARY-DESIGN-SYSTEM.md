<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 20 — COMPONENT LIBRARY & DESIGN SYSTEM

**Suggested filename:** `20-COMPONENT-LIBRARY-DESIGN-SYSTEM.md`  
**Purpose:** Reusable UI components, tokens, variants, accessibility, and usage rules.

## 1. Principles
Use the existing visual direction: dark, cinematic, spacious, concise, and purposeful. The public experience prioritizes the video; admin screens prioritize clarity and speed.

## 2. Design tokens
Reference `04-UI-UX-DESIGN-BRIEF.md`. Maintain one source of truth for color, typography, spacing, radius, shadows, breakpoints, and motion durations.

## 3. Component inventory
| Component | Variants | States | Used in | Owner |
|---|---|---|---|---|
| Button | Primary, secondary, text | Default, hover, focus, disabled, loading | Public/admin | `[owner]` |
| Overlay | Center, left, right | Hidden, entering, visible, exiting | Public | `[owner]` |
| Lead form | Default, success, error | Idle, submitting, success, invalid | Public | `[owner]` |
| Loader | Initial, stalled | Loading, stalled, complete | Public | `[owner]` |
| Admin table | Default, empty | Loading, populated, empty, error | Admin | `[owner]` |

## 4. Component specification template
For each component record purpose, anatomy, variants, properties, responsive behavior, keyboard behavior, screen-reader labels, visual states, examples, and prohibited usage.

## 5. Naming and implementation rules
Define file naming, CSS class conventions, token usage, composition rules, and when a component should remain page-specific.

## 6. Motion
Document duration, easing, reduced-motion alternative, and which movement is scroll-driven. Do not add decorative animation that competes with the video.

## 7. Governance
Describe how a new token or component is proposed, reviewed, tested, documented, and deprecated.


---
