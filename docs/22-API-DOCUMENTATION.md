<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 22 — API DOCUMENTATION

**Suggested filename:** `22-API-DOCUMENTATION.md`  
**Purpose:** Endpoint catalog and exact HTTP contracts for public and admin APIs.

## 1. Conventions
Document base URL, JSON encoding, date format, pagination, request IDs, cache headers, and versioning policy. All examples must use placeholder values, never real secrets or personal data.

## 2. Endpoint index
| Method | Endpoint | Auth | Purpose | Spec section |
|---|---|---|---|---|
| GET | `/api/page/:slug` | Public | Published page configuration | §3 |
| POST | `/api/leads` | Public + abuse controls | Create or deduplicate lead | §4 |
| POST | `/api/events` | Public + allow-list | Record analytics batch | §5 |
| `/api/admin/*` | As defined in Authentication | Editor/admin | Admin operations | §6 |

## 3. Page content endpoint
Document path parameters, response schema, published-only behavior, cache policy, static fallback, and 404 response.

## 4. Lead submission endpoint
Document request fields, required consent, validation, honeypot, rate limit, duplicate behavior, success response, and error responses.

## 5. Analytics endpoint
Document event allow-list, batch size, payload limits, accepted device values, fire-and-forget behavior, and 204 response.

## 6. Admin endpoints
For every route specify method, path, required role, headers, query parameters, request body, response body, status codes, and audit-log side effects. Reference `06-AUTHENTICATION.md` for the authoritative role matrix.

## 7. Error model
| HTTP status | Error code | Meaning | Client action |
|---|---|---|---|
| 400 | `validation_error` | Invalid input | Show field-level message |
| 401 | `unauthenticated` | Missing/invalid session | Sign in again |
| 403 | `forbidden` | Insufficient role | Show access denied |
| 429 | `rate_limited` | Request threshold reached | Wait and retry if appropriate |
| 500 | `internal_error` | Unexpected server failure | Show safe generic message |

## 8. Security and compatibility
Document origin checks, authorization, payload limits, PII redaction, deprecation policy, and contract tests.


---
