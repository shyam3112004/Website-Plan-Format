# 09 — QA Checklist
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Test preparation
Record build/commit, device, browser, viewport, network profile, frame-set version and tester. Test the real production-like preview, not only the Vite development server.

## 2. Functional checks
- [ ] Header/logo links to home and mobile menu opens/closes by keyboard and touch.
- [ ] Scroll sequence starts at first frame and maps progress consistently to frame indices.
- [ ] Reverse scrolling reverses the frame sequence.
- [ ] First, middle, last and scene-transition frames are correct.
- [ ] No missing frame, blank canvas or unexpected stretch.
- [ ] Overlay timing follows the approved storyboard.
- [ ] Products displayed match the company-approved categories.
- [ ] Request a Quote buttons reach the contact form or contact route.
- [ ] Phone links open the dialer and email links open the mail client.
- [ ] Address and all company details match the signed-off content sheet.

## 3. Frame/asset checks
- [ ] Numeric order is correct and numbering is contiguous or explicitly mapped.
- [ ] Manifest count and byte totals match files on disk.
- [ ] All images decode.
- [ ] Desktop and mobile assets are separately tested where supplied.
- [ ] Poster loads before frame sequence.
- [ ] Missing asset fallback does not blank the page.
- [ ] CDN cache behavior is correct for versioned paths.

## 4. Form checks
- [ ] Required fields and one reply channel are validated.
- [ ] Invalid email/phone formats are rejected appropriately.
- [ ] Consent text is visible and understandable.
- [ ] Honeypot/rate limiting work if backend is used.
- [ ] Success state appears only after server confirmation.
- [ ] Failure preserves entered values and presents phone/email fallback.
- [ ] Test enquiry reaches the real approved recipient/database.

## 5. Accessibility checks
- [ ] Keyboard-only navigation and visible focus.
- [ ] Correct heading hierarchy and semantic landmarks.
- [ ] Text contrast checked.
- [ ] Images have meaningful alt text or empty alt for decorative graphics.
- [ ] Canvas has an equivalent text/static experience.
- [ ] `prefers-reduced-motion` shows static content.
- [ ] No auto-playing audio or flashing transitions.
- [ ] Form labels and error announcements work with a screen reader.

## 6. Responsive/device matrix
| Device group | Browser | Required |
|---|---|---|
| Windows laptop | Chrome, Edge, Firefox | Yes |
| macOS laptop | Safari, Chrome | Yes |
| Android phone (mid-range) | Chrome | Yes |
| Android low-memory phone | Chrome | Strongly recommended |
| iPhone | Safari | Yes |
| Tablet | Safari/Chrome | Recommended |

## 7. SEO and content checks
- [ ] Page title, description, canonical URL, OG image and social title are correct.
- [ ] Main products and company details are visible as HTML.
- [ ] Sitemap and robots.txt use the final canonical host.
- [ ] No placeholder copy, lorem ipsum, invented certifications or fake reviews.
- [ ] Privacy/terms links are present where required.
- [ ] Image alt text describes the actual product shown.

## 8. Security checks
- [ ] No service-role key or secret in client bundle.
- [ ] Public API rejects oversized/invalid payloads.
- [ ] Enquiry records are not readable anonymously.
- [ ] Admin endpoints return 401/403 as appropriate if admin exists.
- [ ] Admin route is noindex and no-store.

## 9. Sign-off
| Role | Name | Result | Date |
|---|---|---|---|
| Business owner | `[name]` | Pending | `[date]` |
| Design reviewer | `[name]` | Pending | `[date]` |
| Frontend developer | `[name]` | Pending | `[date]` |
| QA | `[name]` | Pending | `[date]` |
