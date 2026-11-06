# 26 — Accessibility Standard
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Target
Use WCAG 2.2 AA as the engineering target, subject to formal audit. This document does not certify conformance.

## 2. Requirements
- All functions available by keyboard.
- Visible focus indicator and logical focus order.
- Semantic landmarks, headings, lists, links and buttons.
- Sufficient text/control contrast.
- Form labels, required status, instructions and error announcements.
- Meaningful alt text for product and workshop photos.
- Captions/transcript if future video includes spoken information.
- No flashing content or autoplay audio.

## 3. Scroll-driven experience
Canvas animation is supplemental. Provide meaningful HTML headings/body copy and a static poster. Respect `prefers-reduced-motion` and skip frame preload when reduced motion is active. Keep contact CTA reachable without completing the animation. Avoid scroll hijacking and retain native browser scroll behavior.

## 4. Mobile and touch
Support zoom, orientation change and larger text. Controls must have comfortable touch targets and not overlap with overlays. Test viewport changes caused by mobile browser chrome.

## 5. QA
Run automated accessibility checks plus keyboard-only, screen-reader and reduced-motion manual tests. Log every exception with impact, mitigation, owner and expiry.
