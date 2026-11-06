# 31 — Environment Configuration
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Environments
| Environment | Purpose | Data |
|---|---|---|
| Local | Development | Synthetic test enquiries only |
| Preview/staging | QA and stakeholder review | Test data; no real customer enquiries |
| Production | Public company website | Real approved enquiry data |

## 2. Variable register
| Variable | Client/server | Purpose |
|---|---|---|
| `SITE_URL` | Build/server | Canonical public URL |
| `SUPABASE_URL` | Server | Database endpoint if used |
| `SUPABASE_SERVICE_ROLE_KEY` | Server only | Privileged DB connection; never expose |
| `IP_HASH_SALT` | Server only | Salt for abuse-prevention hashing if used |
| `MAIL_PROVIDER_API_KEY` | Server only | Enquiry notification provider |
| `VITE_DEFAULT_PAGE` | Client | Default page slug, non-secret |

Only add variables actually used by the code. Never prefix secrets with `VITE_`.

## 3. Local setup
Copy `.env.example` to `.env.local`, fill only local test credentials, install dependencies and run the project scripts. Keep `.env.local` ignored by Git.

## 4. Validation
Fail the build or API startup when a required variable is missing. Validate URLs and sender address. Do not print secret values in logs.

## 5. Rotation
Rotate credentials after exposure or staff access changes. Update staging first, test, then production. Remove obsolete keys after migration completes.
