# 27 — Performance Engineering
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Goals
The image sequence is the main payload risk. Optimize for first meaningful content, quick access to contact information and stable scroll scrubbing on mid-range devices.

## 2. Budget baseline
| Metric | Initial target | Measurement |
|---|---:|---|
| LCP | < 2.5 s | Lighthouse + field data |
| CLS | < 0.1 | Lighthouse + field data |
| Desktop frames | ≤ 12 MB provisional | Manifest audit |
| Mobile frames | ≤ 5 MB provisional | Manifest audit |
| JS bundle | Set measured target after first build | Bundle analyzer |
| Scroll render | Aim 60 fps; no sustained jank | Real-device profiler |

## 3. Optimization order
1. Show poster and text immediately.
2. Avoid preloading every frame before page becomes useful.
3. Decode keyframes first and draw nearest loaded frame.
4. Produce mobile-appropriate derivatives from existing frames.
5. Use AVIF only after browser support/fallback test.
6. Keep gallery images responsive and lazy-loaded below the fold.
7. Split admin code from public entry.
8. Avoid unnecessary third-party scripts and heavy web fonts.
9. Profile Canvas resize, DPR and memory pressure.

## 4. Measurement protocol
Record frame-set version, browser, device, network, viewport, cache state, build SHA, test date and repeated-run outcomes. Do not publish measured performance claims without this record.

## 5. Regression controls
CI should check build size, asset manifest, missing frames, broken links and Lighthouse thresholds. A frame-set update must include a new payload audit.
