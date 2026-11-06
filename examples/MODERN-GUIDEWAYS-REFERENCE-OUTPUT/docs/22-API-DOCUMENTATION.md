# 22 — API Documentation
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Conventions
Use same-origin HTTPS endpoints, JSON request/response, explicit status codes, bounded payloads and consistent error objects. Only build endpoints that the approved MVP needs.

## 2. Endpoint list
| Method | Path | Purpose | Status |
|---|---|---|---|
| GET | `/api/page/:slug` | Published content config | Optional |
| POST | `/api/enquiries` | Submit quote enquiry | MVP if server storage/notification is used |
| POST | `/api/events` | Allow-listed analytics events | Optional |
| GET | `/api/admin/me` | Validate admin session | Later |
| PUT | `/api/admin/content/:id` | Edit content | Later |
| POST | `/api/admin/content/:id/publish` | Publish content | Later |

## 3. Enquiry request contract
```json
{
  "name": "Required visitor name",
  "companyName": "Optional company",
  "email": "Optional if phone is supplied",
  "phone": "Optional if email is supplied",
  "productInterest": "telescopic-covers",
  "machineType": "Optional owner-approved field",
  "message": "Enquiry message",
  "consent": true,
  "sourcePath": "/contact/"
}
```

The server must require name, product interest, message, consent and at least one reply channel. Maximum lengths and field-specific validation must be set in implementation. Never echo message text to analytics or logs.

## 4. Response contract
Success: `{ "ok": true, "reference": "opaque-id" }`. Error: `{ "ok": false, "error": { "code": "VALIDATION_ERROR", "message": "Please check the highlighted fields." } }`. Do not expose stack traces or provider credentials.

## 5. Security
Apply rate limiting and honeypot validation. Validate product category against the allow-list. Restrict CORS to same origin. Add request size limits and idempotency handling if duplicate submissions appear. Do not permit anonymous reads of enquiry data.

## 6. Contract tests
Test valid request, missing fields, absent reply channel, invalid email/phone, false consent, unsupported category, oversized payload, honeypot, rate limit, database failure and mail-provider failure.
