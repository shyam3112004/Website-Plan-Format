# 12 — Analytics Specification
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Principles
Collect only what helps the company understand whether visitors find product information and contact the business. Avoid collecting form message text in analytics. Configure optional analytics according to the approved privacy notice and consent approach.

## 2. Event catalog
| Event | Trigger | Properties (non-sensitive) |
|---|---|---|
| `page_view` | Page opened | route, referrer category, campaign |
| `hero_started` | Animation enters active stage | frame_set_version |
| `hero_progress` | 25/50/75/100% milestone | milestone, frame_set_version |
| `reduced_motion_view` | Static fallback shown | route |
| `product_section_view` | Product block enters viewport | category_slug |
| `product_cta_click` | Product enquiry click | category_slug |
| `phone_click` | `tel:` link selected | contact_label (not phone value) |
| `email_click` | `mailto:` link selected | contact_label |
| `contact_form_start` | First meaningful form interaction | route |
| `contact_form_success` | Server confirms submission | product_category, route |
| `contact_form_error` | Server returns failure | error_code only |

## 3. Funnel
Visit → hero/story engagement → product section view → CTA action → contact form start → successful enquiry. Phone and email actions should be counted separately; they may be the preferred conversion channel for some visitors.

## 4. Event delivery
Batch non-sensitive events to `/api/events` or an approved analytics provider. Use `sendBeacon` on page exit where appropriate. Validate event names and payload length. Apply rate limiting. Never trust browser events as business records.

## 5. Dashboard metrics
- Visitors and landing routes.
- Scroll milestone reach rate.
- Product-category section views and CTA clicks.
- Phone/email link actions.
- Enquiry form starts, successes and error rate.
- Animation loader stall rate and static fallback usage.
- Device class and browser class (coarse, privacy-aware).

## 6. Quality checks
Test that repeated scroll events are debounced, form payload values are excluded, consent settings are respected and analytics failure never blocks the user journey.

## 7. Retention and ownership
The owner must approve provider, retention period, reporting access and any cookie/consent controls. The analytics owner reviews trends monthly; no user-level behavioral profiling is required for MVP.
