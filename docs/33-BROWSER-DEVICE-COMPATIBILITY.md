<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 33 — BROWSER & DEVICE COMPATIBILITY

**Suggested filename:** `33-BROWSER-DEVICE-COMPATIBILITY.md`  
**Purpose:** Supported browsers, devices, viewport matrix, and fallback behavior.

## 1. Support policy
Use the device matrix in `09-QA-CHECKLIST.md` as the baseline. Record minimum supported browser versions only after product approval.

## 2. Compatibility matrix
| Platform | Browser | Viewport/device | Priority | Last verified |
|---|---|---|---|---|
| macOS | Safari, Chrome | Retina laptop | P0 | `[date]` |
| Windows | Chrome, Edge, Firefox | Laptop | P0 | `[date]` |
| iOS | Safari | iPhone 13+ | P0 | `[date]` |
| Android | Chrome | Pixel 7 / Galaxy A54 | P0 | `[date]` |
| iPadOS | Safari | iPad | P1 | `[date]` |
| Android low-end | Chrome | 4 GB RAM device | P1 | `[date]` |

## 3. Feature support
Document Canvas 2D, AVIF, WebP, `sendBeacon`, `prefers-reduced-motion`, sessionStorage, and required JavaScript features. Define a fallback for each unsupported capability.

## 4. Responsive behavior
Record breakpoints, frame-set choice, crop rules, orientation changes, safe areas, touch behavior, and layout constraints.

## 5. Test protocol
Record actual hardware, OS/browser versions, network, cache state, test date, test results, and reproducible defects.

## 6. Known limitations
List limitations, user impact, workaround, owner, and review date.


---
