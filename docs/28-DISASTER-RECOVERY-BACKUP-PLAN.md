<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 28 — DISASTER RECOVERY & BACKUP PLAN

**Suggested filename:** `28-DISASTER-RECOVERY-BACKUP-PLAN.md`  
**Purpose:** Backup, restore, recovery objectives, incident roles, and verification.

## 1. Critical assets and systems
List GitHub source, Vercel configuration, Supabase production database, Storage assets, domain/DNS, SMTP, and credentials held in the password manager.

## 2. Recovery objectives
| System | RPO | RTO | Approved by | Notes |
|---|---|---|---|---|
| Supabase database | `[decide]` | `[decide]` | `[owner]` | Confirm plan capabilities |
| Public site | `[decide]` | `[decide]` | `[owner]` | Redeploy last known-good build |
| Frame assets | `[decide]` | `[decide]` | `[owner]` | Preserve versioned copies |
| Domain/DNS | `[decide]` | `[decide]` | `[owner]` | Keep registrar access current |

## 3. Backup policy
Specify backup frequency, retention, encryption, access restrictions, off-site copy, backup owner, and evidence of successful completion. Confirm provider-specific backup limits rather than assuming a feature is enabled.

## 4. Restore procedures
Write tested, step-by-step procedures for database restore, static asset restore, Vercel redeploy, DNS recovery, and credential rotation. Include commands only after validating them in staging.

## 5. Incident roles and communications
Define incident commander, technical lead, communications owner, escalation path, status updates, and decision authority.

## 6. Recovery tests
| Test date | Scenario | Restore duration | Data loss | Result | Follow-up |
|---|---|---:|---:|---|---|
| `[date]` | `[scenario]` | `[time]` | `[amount]` | `[pass/fail]` | `[ticket]` |

## 7. Post-incident review
Capture timeline, root cause, impact, recovery actions, prevention, and assigned owners.


---
