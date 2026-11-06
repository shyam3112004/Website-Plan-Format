# 13 — Domain, DNS & Email
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Current domain information
The supplied profile prints `www.modernguideways.com`. Verify that the company owns the domain and controls the registrar and DNS account. Choose one canonical host (`https://www.modernguideways.com` or the apex host) and redirect the other to it.

## 2. Domain checklist
- [ ] Registrar account owner is the company, not an individual contractor.
- [ ] MFA and registrar lock are enabled.
- [ ] Renewal and billing contacts are current.
- [ ] DNS access is documented.
- [ ] Canonical www/apex choice is recorded.
- [ ] HTTPS certificate is issued and renewal is automatic.

## 3. DNS deployment
Use the exact records requested by the selected hosting provider. Do not copy guessed A/CNAME records into production. Save existing records before modifying them. Lower TTL before cutover, verify DNS and HTTPS, then restore the normal TTL after stabilization.

## 4. Business email
The profile lists `modernguideways@gmail.com`. Confirm whether this is the preferred public enquiry inbox. If the company later uses a domain mailbox, migrate only after testing delivery and setting up provider-recommended SPF, DKIM and DMARC records.

## 5. Form delivery
Choose the primary route: server-side email notification, Supabase-stored enquiry with notification, or both. Test spam-folder placement, sender identity, reply-to address and bounce handling. Never put SMTP credentials in front-end JavaScript.

## 6. Verification
Test `www` and apex, HTTP-to-HTTPS redirect, email delivery, SPF/DKIM/DMARC results if configured, contact links, and the final canonical URL in SEO metadata.

## 7. Renewal and risk
Record domain, hosting, mailbox and database renewal owners. Set reminders before expiry. Keep the company in control of all accounts.
