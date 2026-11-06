<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 23 — INTEGRATION DOCUMENTATION

**Suggested filename:** `23-INTEGRATION-DOCUMENTATION.md`  
**Purpose:** External service integrations, credentials, event flows, retries, and ownership.

## 1. Integration inventory
| Service | Purpose | Environment | Credential owner | Criticality |
|---|---|---|---|---|
| Vercel | Hosting and serverless functions | Preview/production | DevOps | Critical |
| Supabase | Database, Auth, Storage | Staging/production | DevOps | Critical |
| GitHub | Source control and CI | Repository | Tech lead | Critical |
| SMTP provider | Sign-in email delivery | Production | DevOps | High |
| Analytics provider | Optional external analytics | If enabled | Marketing | Optional |

## 2. Per-integration specification
Record business purpose, data exchanged, endpoints/scopes, authentication method, secret storage, environments, rate limits, timeouts, retries, idempotency, webhook verification, failure behavior, support contact, and offboarding procedure.

## 3. Data-flow and privacy review
Map each field sent to each provider. Confirm data minimization, retention, region, data-processing agreement, and privacy-policy disclosure before enabling a service.

## 4. Failure and recovery matrix
| Failure | User impact | Detection | Fallback | Owner |
|---|---|---|---|---|
| Supabase unavailable | Dynamic content/API may fail | API errors | Last-good static content where supported | DevOps |
| SMTP delivery delayed | Editor cannot receive sign-in link promptly | Delivery metrics | Provider support / resend policy | DevOps |
| Analytics unavailable | Metrics delayed/lost | Delivery monitoring | Site continues; never block scroll | Frontend |

## 5. Change management
Require sandbox testing, secret rotation, staging verification, production approval, and rollback notes for integration changes.


---
