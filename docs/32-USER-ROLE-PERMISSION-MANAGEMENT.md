<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 32 — USER, ROLE & PERMISSION MANAGEMENT

**Suggested filename:** `32-USER-ROLE-PERMISSION-MANAGEMENT.md`  
**Purpose:** User lifecycle and authorization governance for editors and admins.

## 1. Scope
The current product supports invited editors and admins only. Visitor accounts are explicitly out of scope in the PRD.

## 2. Role matrix
Use `06-AUTHENTICATION.md` as the source of truth. Copy the approved capabilities into a maintained matrix and do not create conflicting permissions here.

## 3. User lifecycle
Document invitation, email verification/magic link, first login, MFA enrollment, role change, temporary suspension, disablement, reactivation, and removal.

## 4. Permission enforcement
Every privileged operation must be checked server-side. Hiding a UI control is not authorization. Specify deny-by-default behavior and tests for each role and endpoint.

## 5. Access reviews
| Review cadence | Reviewer | Check | Evidence |
|---|---|---|---|
| Quarterly | Admin / owner | Active accounts and least privilege | Signed review |
| Staff departure | Admin | Disable account and revoke sessions | Audit record |
| Role change | Admin | Confirm new permissions | Audit record |

## 6. Audit requirements
Record actor, action, target entity, timestamp, and safe before/after diff. Exclude secrets and unnecessary personal data.

## 7. Break-glass access
Define emergency admin access, approval, credential protection, monitoring, and post-use review.


---
