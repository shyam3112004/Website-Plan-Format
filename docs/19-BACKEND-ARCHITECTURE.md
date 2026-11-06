<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 19 — BACKEND ARCHITECTURE

**Suggested filename:** `19-BACKEND-ARCHITECTURE.md`  
**Purpose:** Server-side layers, request lifecycle, data access, validation, authorization, and failure handling.

## 1. Purpose and scope
Describe the server-side responsibilities hosted as Vercel Serverless Functions and backed by Supabase.

## 2. Architecture diagram
Add a Mermaid diagram showing browser → Vercel API functions → validation/authorization → Supabase Postgres/Auth/Storage. Mark trust boundaries and secret-bearing components.

## 3. Backend components
| Component | Responsibility | Runtime | Source location |
|---|---|---|---|
| Page content API | Return published page configuration | Vercel Function | `api/page/` |
| Lead API | Validate consent and store lead | Vercel Function | `api/leads` |
| Events API | Accept allow-listed analytics batches | Vercel Function | `api/events` |
| Admin API | Enforce role checks and perform admin actions | Vercel Function | `api/admin/` |

## 4. Request lifecycle
For each route document parsing, schema validation, authentication, authorization, database operation, response serialization, logging, and error mapping.

## 5. Data access
Document use of the Supabase service-role key on the server only, connection behavior, query limits, transactions, indexes, and migration ownership. Explain why browser clients do not access Postgres directly.

## 6. Authentication and authorization
Reference `06-AUTHENTICATION.md`. Define public, editor, and admin route classes and deny-by-default behavior.

## 7. Validation and abuse prevention
Specify input schemas, maximum payload sizes, rate limits, honeypot behavior, duplicate handling, CORS/origin policy, and safe error messages.

## 8. Reliability
Define timeouts, retries (only where safe), idempotency, database outage behavior, static content fallback, and degraded-mode behavior.

## 9. Observability
Specify structured log fields, request IDs, error reporting, alert thresholds, and sensitive-data redaction.

## 10. Capacity and scaling
Record expected traffic, concurrency assumptions, database limits, function limits, and load-test results.

## 11. Open decisions
Track unresolved architecture decisions with owner, impact, and due date.


---
