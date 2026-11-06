<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 31 — ENVIRONMENT CONFIGURATION

**Suggested filename:** `31-ENVIRONMENT-CONFIGURATION.md`  
**Purpose:** Environment separation, configuration variables, secret handling, and validation.

## 1. Environments
Use the existing Local, Preview, and Production model in `08-DEPLOYMENT-RUNBOOK.md`. Preview must use staging Supabase; production must use production Supabase.

## 2. Variable register
| Variable | Client/server | Required in | Secret? | Purpose |
|---|---|---|---|---|
| `SITE_URL` | Build/server | All deployed | No | Canonical site URL |
| `SUPABASE_URL` | Build/server | All | No | Supabase project URL |
| `SUPABASE_ANON_KEY` | Client if used | Relevant env | No (public key) | Supabase client auth |
| `SUPABASE_SERVICE_ROLE_KEY` | Server/build only | All deployed | **Yes** | Privileged server access |
| `VITE_PAGE_SLUG` | Client | All | No | Page slug |

Reconcile this table against the authoritative `.env.example` and TRD before use. Never expose server-only secrets through `VITE_` variables.

## 3. Secret management
Specify approved secret store, access roles, rotation schedule, emergency rotation, audit evidence, and removal from logs/history.

## 4. Configuration validation
Define startup/build-time checks for missing values, malformed URLs, wrong environment/project pairing, and unsafe production defaults.

## 5. Local setup
Document how developers obtain access, create `.env.local`, run local services, and verify they are connected to staging.

## 6. Change procedure
Require update to `.env.example`, deployment settings, documentation, and validation in preview when variables change.


---
