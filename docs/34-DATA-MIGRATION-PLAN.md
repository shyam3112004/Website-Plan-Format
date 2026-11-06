<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 34 — DATA MIGRATION PLAN

**Suggested filename:** `34-DATA-MIGRATION-PLAN.md`  
**Purpose:** Database and content migrations, compatibility, verification, and rollback.

## 1. Scope and principles
Database migrations are versioned, ordered, reviewed, and applied to staging before production. Do not edit a migration that has already been applied; add a new numbered migration.

## 2. Migration inventory
| Version | File | Change | Dependencies | Environments | Status |
|---|---|---|---|---|---|
| 001 | `001_init.sql` | Initial schema | None | Staging, production | Existing |
| 002 | `002_auth.sql` | Editors and audit log | Supabase Auth | Staging, production | Planned/current plan |

## 3. Migration design
Document schema changes, data transformations, indexes, constraints, defaults, backfill strategy, expected locks/downtime, and compatibility with the currently deployed code.

## 4. Execution procedure
Include backup verification, staging apply, test suite, row-count/data checks, approval, production apply, deployment order, and post-migration monitoring.

## 5. Rollback and forward-fix
State whether each migration is reversible. Prefer a tested forward-fix when destructive rollback is unsafe. Document recovery from partial application.

## 6. Data verification
Define pre/post counts, null checks, uniqueness checks, sample records, referential integrity, and application smoke tests.

## 7. Content migration
If pages, scenes, overlays, or frame-set paths change, document export/import format, mapping, validation, and publishing steps.

## 8. Evidence
Store migration owner, commit, timestamp, environment, result, approver, and follow-up ticket.


---
