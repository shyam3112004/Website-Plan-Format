# 33 — Browser & Device Compatibility
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Support policy
Support current and recent major versions of Chrome, Edge, Firefox and Safari. Final minimum browser versions should be agreed after implementation testing; do not promise legacy support that the team cannot test.

## 2. Required coverage
| Device | Browser | Important checks |
|---|---|---|
| Windows desktop | Chrome/Edge/Firefox | Canvas rendering, fonts, anchors, form |
| macOS desktop | Safari/Chrome | ScrollTrigger, sticky stage, resize |
| Android mid-range | Chrome | Memory, decode, touch scroll, battery/performance |
| Low-memory Android | Chrome | Progressive loading and poster fallback |
| iPhone | Safari | Dynamic viewport, address-bar collapse, touch scroll |
| Tablet | Safari/Chrome | Orientation and responsive crop |

## 3. Feature fallbacks
If Canvas or image decoding fails, use poster and HTML content. If smooth scrolling conflicts with browser behavior, disable Lenis and use native scrolling. If AVIF fails, use WebP. If no mobile frame set exists, use the approved tested fallback.

## 4. Manual test protocol
Test 320px/360px/390px mobile widths, tablet portrait/landscape, 1366px desktop and large desktop. Rotate device during loading; scroll both directions; zoom text; change motion preference; throttle network and CPU.
