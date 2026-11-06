# 06 — Authentication
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Scope
There are no visitor accounts in the MVP. Authentication is for company-approved editors and administrators only, and may be deferred until the company needs a custom editor dashboard. The first release can use repository-managed JSON content or a restricted Supabase dashboard with access granted only to named owners.

## 2. Roles
| Role | Public site | Edit page/product copy | Publish | View enquiries | Manage users/settings |
|---|---|---|---|---|---|
| Visitor | Yes | No | No | No | No |
| Editor | Yes | Yes, draft only | As approved | No by default | No |
| Admin | Yes | Yes | Yes | Yes | Yes |

## 3. Authentication design (later release)
- Supabase Auth invite-only sign-in, preferably magic link or passwordless email.
- No public sign-up route.
- Admin role requires multi-factor authentication if supported by the chosen provider configuration.
- All privileged API routes validate the bearer token and role server-side.
- The browser may contain only the public/anon key. Service-role secrets remain in server environment variables.

## 4. Required API protections
- `401` for missing/invalid token.
- `403` for valid identity without required role.
- `no-store` for private admin responses.
- `noindex, nofollow` on all admin pages.
- Audit log entry for each content publish, user-role change, enquiry export or deletion.

## 5. Account lifecycle
Admin invites an approved work email → user accepts invite → MFA setup where required → access reviewed quarterly → access revoked immediately when no longer needed. Do not share admin logins.

## 6. Recovery
Keep at least two named administrators. Document secure account recovery and key rotation. Do not put passwords, recovery codes or API keys in project Markdown files or screenshots.

## 7. Release gate
Do not launch `/admin` until role checks, 401/403 tests, MFA, audit logging, account offboarding and a recovery test have passed.
