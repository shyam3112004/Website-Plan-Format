# 19 — Backend Architecture
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Backend principles
The site is primarily informational. Keep backend scope small and avoid a full customer portal. Use Vercel serverless functions for narrow endpoints and Supabase only for content/enquiry storage that the owner wants persisted.

## 2. Component map
- Public static hosting: page HTML, CSS, JS, frames and gallery files.
- Page-config endpoint: published content only, optional for MVP.
- Enquiry endpoint: validates, rate-limits and stores/routes enquiry.
- Analytics endpoint: accepts allow-listed event types only.
- Admin endpoints: later, authenticated and role checked.
- Supabase: Postgres and Auth if approved.

## 3. Request lifecycle
Browser validates basic form shape → sends same-origin request → server validates required fields and size → anti-spam checks → save enquiry or send notification → return opaque reference/success state. Never return other enquiries to the public browser.

## 4. Reliability
- Static fallback for page content.
- Retry-safe enquiry operation with idempotency key if duplicate submissions are possible.
- Explicit timeout and safe error message.
- Monitoring for provider/API failure.
- Backups and restore test when database storage is enabled.

## 5. Scaling
Public assets should be CDN-delivered. Keep frame files immutable/versioned. Database load should remain small for an enquiry-led site. Add queues/CRM integrations only when measured need exists.

## 6. Open decisions
Confirm whether submissions go to email, database or both; notification ownership; retention; CAPTCHA threshold; and whether the company needs an internal enquiry dashboard.
