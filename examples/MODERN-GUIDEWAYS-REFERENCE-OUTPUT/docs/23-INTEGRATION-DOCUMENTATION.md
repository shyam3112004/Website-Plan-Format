# 23 — Integration Documentation
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Integration inventory
| Integration | Purpose | Required for MVP | Owner |
|---|---|---|---|
| Vercel | Hosting/CDN/functions | Yes, if chosen stack retained | DevOps |
| Supabase | Database/auth | Only if enquiry/content storage needs it | Backend owner |
| Email provider | Enquiry notifications | Yes if form submits by email | Business/DevOps |
| Analytics provider | Site measurement | Optional | Marketing owner |
| Search Console | Search indexing/monitoring | Recommended | SEO owner |
| Map provider/embed | Location directions | Optional; do not block contact details | Business owner |

## 2. Per-integration acceptance
Document account owner, data fields exchanged, credentials location, webhook/API behavior, retry policy, rate limit, failure mode, test evidence and offboarding path.

## 3. Privacy review
For every third party, identify personal data transmitted, purpose, region/storage, retention, subprocessors and contractual terms. Do not embed third-party maps, videos or tracking until their privacy impact is reviewed.

## 4. Failure and recovery
- Email service unavailable: preserve enquiry in approved storage or display phone/email alternative.
- Database unavailable: show safe failure and do not claim the enquiry was received.
- Analytics unavailable: continue the website without blocking visitors.
- Search indexing unavailable: site remains usable; retry verification later.

## 5. Change management
Test provider changes in staging and update API, privacy and operational docs. Rotate credentials when an integration owner changes.
