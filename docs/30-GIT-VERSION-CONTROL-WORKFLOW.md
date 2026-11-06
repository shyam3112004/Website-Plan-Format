<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 30 — GIT & VERSION CONTROL WORKFLOW

**Suggested filename:** `30-GIT-VERSION-CONTROL-WORKFLOW.md`  
**Purpose:** Branching, commits, pull requests, code review, releases, and rollback discipline.

## 1. Repository rules
Record repository URL, default branch, owners, branch protection, required checks, and access review cadence.

## 2. Branch model
| Branch | Purpose | Deployment |
|---|---|---|
| `main` | Production-ready code | Production |
| `feature/*` | Feature development | Preview |
| `fix/*` | Bug fixes | Preview |
| `release/*` | Optional release stabilization | Preview / controlled |

## 3. Commit conventions
Define a consistent commit format, examples, scope naming, and rules for generated assets, migrations, secrets, and large source video files.

## 4. Pull request checklist
- [ ] Linked requirement or issue
- [ ] Scope and screenshots described
- [ ] Tests added and passing
- [ ] Accessibility and responsive behavior checked
- [ ] No secrets or personal data committed
- [ ] Database migration reviewed for compatibility
- [ ] Performance and asset budgets checked
- [ ] Documentation updated

## 5. Review and merge
Specify minimum reviewers, code-owner approval, CI checks, merge strategy, and emergency exception process.

## 6. Release and rollback
Document tags, changelog update, deployment approval, smoke tests, rollback trigger, and rollback owner.

## 7. Repository hygiene
Document dependency updates, vulnerability alerts, stale branch cleanup, and source-video/license file handling.


---
