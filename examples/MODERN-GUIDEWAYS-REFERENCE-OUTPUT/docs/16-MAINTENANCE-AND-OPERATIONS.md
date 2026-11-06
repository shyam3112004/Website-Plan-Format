# 16 — Maintenance & Operations
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Ownership register
Maintain named owners for domain/DNS, Vercel, Git repository, Supabase (if used), public email, enquiry handling, content approvals and legal notices. Use MFA and remove access when a person leaves the project.

## 2. Routine tasks
| Cadence | Task | Owner |
|---|---|---|
| Weekly | Check enquiry delivery and respond to pending enquiries | Business enquiry owner |
| Weekly | Check uptime and error alerts | Technical owner |
| Monthly | Review analytics, broken links, product content and contact details | Marketing/business owner |
| Monthly | Review dependency updates and security advisories | Developer |
| Quarterly | Test backup restore, review accounts and rotate credentials where due | Admin/DevOps |
| Annually | Review domain renewal, privacy/terms, company profile and asset rights | Business owner |

## 3. Content operations
Keep a source-of-truth content sheet with approved wording and dates. A product category change, contact update or experience claim must be reviewed. Do not directly edit production JSON without version control.

## 4. Frame updates
Maintain versioned frame directories. Run audit, build contact sheet, compare to live storyboard, test mobile, deploy new manifest and retain old version for rollback. Never overwrite live immutable URLs.

## 5. Dependency and security maintenance
Update packages in a branch, run tests, inspect bundle changes and review security warnings. Do not run unreviewed major upgrades directly in production.

## 6. Cost register
Record current plan, usage, invoices and renewal date for hosting, domain, email, database, analytics and monitoring. Provider pricing and free-tier limits can change; verify from provider dashboards.

## 7. End-of-life
If the site is retired, preserve company-owned source assets and data according to retention policy, redirect the domain to an approved destination, revoke keys and user access, and document deletion of third-party services.
