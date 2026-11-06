# 18 — Frontend Architecture
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Scope
Define a static-first multi-section company website with one scroll-driven canvas experience. Keep the renderer independent from business content and product cards.

## 2. Suggested source structure
```text
src/
  main.js
  content/
    contentLoader.js
    home.json
  animation/
    frameManifest.js
    frameLoader.js
    scrollCanvas.js
    overlays.js
    scrollController.js
  components/
    header.js
    mobileMenu.js
    productCard.js
    gallery.js
    contactForm.js
    footer.js
  services/
    enquiryApi.js
    analytics.js
  styles/
    tokens.css
    base.css
    layout.css
    components.css
    motion.css
  admin/                 # later release only
```

## 3. Rendering lifecycle
DOMContentLoaded → read motion preference → load static content → choose poster/animation mode → fetch frame manifest → decode keyframes → unlock scroll → connect ScrollTrigger → render nearest frame on update → cleanup listeners/bitmaps when needed.

## 4. Canvas behavior
Use cover-fit math so the image fills the stage without stretching. Keep subject safe area in mind. Set intrinsic canvas size to CSS dimensions × capped DPR. Do not resize the canvas on every scroll tick; resize only when viewport dimensions change.

## 5. Navigation and page structure
Use normal semantic anchors for page sections and routes. Do not replace browser navigation with custom scroll logic. On mobile, menu state should be focus-managed and close on navigation/escape.

## 6. Failure boundaries
If frame loading fails, hide canvas and display poster. If animation JS fails, content sections and contact links still work. If API fails, use local approved JSON fallback.

## 7. Testing
Unit-test frame index calculations, numeric file ordering, responsive frame selection and overlay windows. E2E-test forward/back scroll, resize, reduced motion, mobile menu, CTA and enquiry form.
