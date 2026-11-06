<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 29 — LOGGING, MONITORING & OBSERVABILITY

**Suggested filename:** `29-LOGGING-MONITORING-OBSERVABILITY.md`  
**Purpose:** Operational signals, dashboards, alerting, log retention, and incident diagnosis.

## 1. Observability goals
Detect broken visitor journeys, API failures, slow frame loading, database issues, authentication failures, and deployment regressions without collecting unnecessary personal data.

## 2. Signals
| Signal | Source | Dashboard | Alert threshold | Owner |
|---|---|---|---|---|
| HTTP 5xx rate | Vercel functions | `[link]` | `[define]` | DevOps |
| Page/API availability | Uptime monitor | `[link]` | `[define]` | DevOps |
| Loader stall rate | Analytics events | `[link]` | `[define]` | Frontend |
| Lead submission failures | API logs / metrics | `[link]` | `[define]` | Backend |
| Auth/MFA failures | Supabase Auth | `[link]` | `[define]` | Admin |
| Database health | Supabase | `[link]` | `[define]` | DevOps |

## 3. Structured logging
Define timestamp, severity, environment, service, route, request ID, duration, status, and safe error code. Never log access tokens, service-role keys, raw lead emails, or free-form personal data.

## 4. Alerts and escalation
For each alert specify trigger, severity, deduplication, notification destination, on-call owner, response target, and runbook link.

## 5. Dashboards
Maintain separate views for public experience, APIs, database, authentication, deployments, and cost.

## 6. Retention and privacy
Set retention by log category, restrict access, redact identifiers, and align with `11-PRIVACY-AND-LEGAL.md`.

## 7. Operational review
Review alert noise, incidents, capacity, costs, and service-level trends monthly.


---
