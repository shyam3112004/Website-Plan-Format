<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 15 — Launch And Go Live

# Launch & Go-Live
**Related:** [PRD](./01-PRD.md) §12 · [Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md) §5–7, §12 · [QA Checklist](./09-QA-CHECKLIST.md) · [Domain, DNS & Email](./13-DOMAIN-DNS-EMAIL.md) · [Implementation Plan](./17-IMPLEMENTATION-PLAN.md) §4

## 1. Roles
R = does the work, A = accountable (one person), C = consulted, I = informed.

| Activity | PM | Designer | Frontend | Backend / DevOps | QA | Legal / Counsel | Marketing / Editor | Stakeholder |
|---|---|---|---|---|---|---|---|---|
| Go / No-Go decision | R | I | C | C | C | C | C | A |
| QA sign-off | I | I | C | C | A/R | | | I |
| Legal checklist ([Privacy §9](./11-PRIVACY-AND-LEGAL.md)) | A | | | C | | R | I | I |
| DNS cutover | A | | I | R | C | | I | I |
| Smoke test and monitoring | I | | C | A/R | C | | | |
| Launch communications | A | | | | | | R | C |
| 72-hour watch | A | | C | R | C | | C | I |
| Day-7 / day-30 review | A/R | C | C | C | | | C | C |

## 2. Timeline
`L` is launch day. The plan's milestones are M4 (release candidate and Go/No-Go) and M5 (launch, 2026-11-27) in the [Implementation Plan](./17-IMPLEMENTATION-PLAN.md).

| When | What | Owner |
|---|---|---|
| L − 14 days | Domain and email set up ([Domain §2–5](./13-DOMAIN-DNS-EMAIL.md)); legal drafts with counsel | DevOps, PM |
| L − 10 | Content freeze; editors trained ([Editor Guide](./10-CONTENT-EDITOR-GUIDE.md)) | PM |
| L − 7 | Release candidate on a preview URL; full [QA Checklist](./09-QA-CHECKLIST.md) run | QA |
| L − 5 | Fix all P0 and P1 findings; re-test | FE, BE |
| L − 3 | **Go / No-Go meeting** (§3) | PM, Stakeholder |
| L − 2 | Lower DNS TTL to 300 s ([Domain §7](./13-DOMAIN-DNS-EMAIL.md)); apply production migrations | DevOps |
| L − 1 | Production deploy to the Vercel URL (domain not switched yet); smoke test (§4 step 3) | DevOps |
| **L** | DNS cutover (§4); launch communications (§7) | DevOps, Marketing |
| L + 1 to L + 3 | 72-hour watch (§6) | DevOps |
| L + 7 | Review (§8); raise DNS TTL | PM |
| L + 30 | Review (§8); retro; backlog | PM |

> **Check the launch day.** The plan's M5 date (2026-11-27) is a Friday, and in the US it is the Friday after Thanksgiving. A Friday launch puts most of the 72-hour watch on a weekend, and busy shopping days can crowd out your launch message. Unless someone is on call all weekend, prefer a Tuesday or Wednesday. If you move it, move M4 and M5 together.

## 3. Go / No-Go
Every line must be **Yes**. One **No** means no-go until it is fixed or the Stakeholder accepts the risk in writing.

| # | Criterion | Evidence |
|---|---|---|
| 1 | All P0 requirements done; no open P0 or P1 bugs | Issue tracker |
| 2 | QA sign-off block complete | [QA Checklist §12](./09-QA-CHECKLIST.md) |
| 3 | Lighthouse: Performance ≥ 85, Accessibility ≥ 95, SEO ≥ 95; budgets pass | Lighthouse CI report |
| 4 | Legal checklist complete; policy pages live on the preview | [Privacy & Legal §9](./11-PRIVACY-AND-LEGAL.md) |
| 5 | Production environment ready: env vars set, migrations applied, retention jobs created | [Runbook §2–3, §8](./08-DEPLOYMENT-RUNBOOK.md) |
| 6 | Domain registered, DNS prepared, certificate ready, mailboxes and email authentication working | [Domain §2–5](./13-DOMAIN-DNS-EMAIL.md) |
| 7 | SEO assets in place (sitemap, robots, icons, 404) | [SEO §11](./14-SEO-AND-DISCOVERABILITY.md) |
| 8 | Monitoring and alerts set; the on-call person for the next 72 hours is named | [Runbook §9](./08-DEPLOYMENT-RUNBOOK.md) |
| 9 | Rollback tested on the preview or staging deployment | [Runbook §7](./08-DEPLOYMENT-RUNBOOK.md) |
| 10 | Editors can change copy unaided | [Editor Guide](./10-CONTENT-EDITOR-GUIDE.md) |
| 11 | Stakeholder approved the visual QA | Written approval |

Record the decision, the date, who attended and any accepted risks.

## 4. DNS Cutover
Run in this order. The person who runs each step ticks it.

| # | Step | Done |
|---|---|---|
| 1 | TTL on the records to be changed is 300 s (set at L − 2) | ☐ |
| 2 | Production `main` is deployed; `SITE_URL` is the final domain | ☐ |
| 3 | Smoke test on the Vercel URL passes ([Runbook §7](./08-DEPLOYMENT-RUNBOOK.md)) | ☐ |
| 4 | Domains added in Vercel, canonical host chosen, redirect set ([Domain §4](./13-DOMAIN-DNS-EMAIL.md)) | ☐ |
| 5 | If the domain served an old site: redirect map ready and tested (table below) | ☐ |
| 6 | Change the A and CNAME records. Keep a copy of the old values. | ☐ |
| 7 | Confirm resolution with `dig` and that the certificate is issued | ☐ |
| 8 | Smoke test again on the real domain | ☐ |
| 9 | Supabase redirect allow-list and Auth site URL use the final domain | ☐ |
| 10 | Uptime monitor and alerts point at the real domain | ☐ |
| 11 | Search Console and Bing verified; sitemap submitted ([SEO §10](./14-SEO-AND-DISCOVERABILITY.md)) | ☐ |
| 12 | Share a link in a chat app and on one social network and check the preview | ☐ |
| 13 | Announce (§7) | ☐ |

Redirect map for an existing site (permanent redirects only):

| Old URL | New URL | Tested |
|---|---|---|
| `/old-page` | `/` or the closest page | ☐ |

Keep the old hosting running for at least 7 days so a DNS rollback is possible.

## 5. Rollback Triggers
Decide these before launch, so nobody debates them during an incident.

| Trigger | Action |
|---|---|
| A P0 bug with no fix within 2 hours | Roll back the deployment ([Runbook §7](./08-DEPLOYMENT-RUNBOOK.md)) |
| API 5xx rate above 5% for 15 minutes | Investigate; roll back the deployment if it started with the release |
| Lead form failing for all users | Fix or roll back immediately. Leads are the main conversion. |
| Site unreachable or wrong certificate after the cutover | Restore the old DNS values (they propagate within the 300 s TTL) |
| Personal data exposed | Follow [Runbook §12](./08-DEPLOYMENT-RUNBOOK.md) and [Privacy & Legal §8](./11-PRIVACY-AND-LEGAL.md) |

## 6. 72-Hour Watch
One named person owns each check. Write down what you saw each time, even if it is "all normal".

| When | Check | Look for |
|---|---|---|
| L + 1 h | Smoke test; uptime monitor; error logs | Any 5xx, console errors, certificate warnings |
| L + 1 h | Submit a test lead, then delete it | Works end to end |
| L + 4 h | `analytics_events` volume by event name | `page_view` growing; `loader_stalled` rate normal ([Analytics §5](./12-ANALYTICS-SPEC.md)) |
| L + 24 h | Lead count; function error rate; database usage | Leads arriving; no limit warnings |
| L + 24 h | Real device check (one iPhone, one Android) | Scrub smooth; loader behaves |
| L + 48 h | Search Console: coverage, sitemap status | Pages discovered, no errors |
| L + 72 h | Summary note to the team | Issues found, fixed and open |

## 7. Communications
| Audience | What | Owner | Link rule |
|---|---|---|---|
| Internal team | Launch message with the URL and who to contact | PM | |
| Stakeholders | Go-live confirmation and the first-day numbers | PM | |
| Customers and social followers | Announcement | Marketing | Every link has UTM parameters ([SEO §9](./14-SEO-AND-DISCOVERABILITY.md)) |
| Partners and press | Short note with the OG preview checked | Marketing | UTM `medium=referral` |
| Support mailbox | Draft replies for common questions | Marketing | |

Keep the announcement for after DNS and the smoke test (§4 step 13).

## 8. Day-7 and Day-30 Review
Use the SQL in the [Analytics Spec §5](./12-ANALYTICS-SPEC.md).

| Goal | Measure | Target | Result |
|---|---|---|---|
| G1 Engagement | Average scroll depth | ≥ 70% | |
| G2 Conversion | CTA conversion | ≥ 3% | |
| G3 Speed | LCP, median loader time, scrub smoothness | LCP < 2.5 s; loader ≤ 3 s desktop / ≤ 4 s mobile | |
| G4 Easy updates | Copy changes made without a developer | At least one | |
| G5 Safe management | Access review; no unexplained admin actions in `audit_log` (v1.1) | Clean | |

Then hold a short retro: what worked, what slipped, what to change. Put follow-ups in the backlog and note scope decisions in the [CHANGELOG](../CHANGELOG.md).

## 9. Handover
Complete before the project team stands down.

| Item | Done |
|---|---|
| Access register complete, every account has two named admins ([Maintenance §1](./16-MAINTENANCE-AND-OPERATIONS.md)) | ☐ |
| Cost register and renewal calendar filled in ([Maintenance §2, §7](./16-MAINTENANCE-AND-OPERATIONS.md)) | ☐ |
| Routine tasks scheduled with owners ([Maintenance §3](./16-MAINTENANCE-AND-OPERATIONS.md)) | ☐ |
| Editors trained; Editor Guide handed over | ☐ |
| On-call and support contacts published | ☐ |
| Documentation set stored in the repo and up to date | ☐ |

---
