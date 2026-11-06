# 11 — Privacy & Legal
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Purpose
Document what the website collects, why, who can access it and how requests are handled. This is an implementation checklist, not legal advice. Have the company’s legal adviser review public policies and the applicable Indian privacy/data-protection requirements before launch.

## 2. Data inventory
| Data | Source | Purpose | Access |
|---|---|---|---|
| Name | Enquiry form | Reply to enquiry | Approved enquiry handlers |
| Company name | Optional form field | Understand business context | Approved enquiry handlers |
| Email / phone | Enquiry form | Reply to enquiry | Approved enquiry handlers |
| Product interest and message | Enquiry form | Understand requirement | Approved enquiry handlers |
| Consent timestamp | Form submission | Evidence of notice/consent | Admin/system |
| IP hash / abuse signal | Server, if needed | Rate-limit and abuse prevention | Restricted system access |
| Analytics events | Browser, subject to policy | Understand site use | Analytics owner |

Do not collect identity documents, payment data or unrelated personal details.

## 3. Enquiry notice
Before submission, explain that the visitor is providing details so Modern Guideways can respond to the stated enquiry. Identify the contact channel for privacy questions. Do not make newsletter marketing consent a condition of requesting a quote unless separately justified and reviewed.

## 4. Retention and rights
The business must choose a retention schedule for enquiries, backups and analytics before launch. Provide a contact route for access/correction/deletion requests and document identity verification and response handling. Do not claim a specific statutory response period until legal review confirms it.

## 5. Cookies and analytics
Avoid non-essential cookies by default. If analytics, advertising or embedded third-party media introduces tracking, disclose it and obtain consent where required. Prefer privacy-minimal events; do not send form message contents into analytics.

## 6. Public legal pages
`/privacy/` should identify the business/contact, data categories, purposes, sharing/processors, retention, security approach and request channel. `/terms/` should explain site content use, accuracy limitations, third-party links and enquiry/quotation disclaimers. Both require legal review.

## 7. Image and video rights
Confirm the company owns or is licensed to use all video frames, product photographs, logo assets, fonts and any third-party footage. Maintain the original project files and rights evidence in restricted storage.

## 8. Incident handling
Restrict access, preserve logs, rotate affected credentials, assess data exposure, notify company leadership and obtain legal guidance on required external notifications. Record the incident and corrective actions.

## 9. Pre-launch checklist
- [ ] Business controller/contact details approved.
- [ ] Privacy notice and terms reviewed.
- [ ] Enquiry consent language approved.
- [ ] Third-party processors recorded.
- [ ] Retention and deletion process assigned.
- [ ] Analytics/cookie behavior reviewed.
- [ ] Asset ownership and licenses verified.
