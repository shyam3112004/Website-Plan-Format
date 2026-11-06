<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 16 — Maintenance And Operations

# Maintenance & Operations
**Related:** [Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md) §7–12 · [Domain, DNS & Email](./13-DOMAIN-DNS-EMAIL.md) · [Launch & Go-Live](./15-LAUNCH-AND-GO-LIVE.md) §9 · [Privacy & Legal](./11-PRIVACY-AND-LEGAL.md) · [Authentication](./06-AUTHENTICATION.md)

## 1. Ownership and Access
Every account has an owner and a backup, so one person leaving never locks the team out.

| Account | Owner | Backup | 2FA | Notes |
|---|---|---|---|---|
| Domain registrar | `[name]` | `[name]` | ☐ | Company-owned ([Domain §2](./13-DOMAIN-DNS-EMAIL.md)) |
| DNS provider (if separate) | | | ☐ | |
| Vercel | | | ☐ | Team plan, not a personal account |
| Supabase (staging, production) | | | ☐ | |
| GitHub | | | ☐ | Branch protection on `main` |
| Mailbox provider | | | ☐ | |
| Transactional email / SMTP | | | ☐ | |
| Uptime monitoring | | | ☐ | |
| Search Console, Bing Webmaster | | | ☐ | |
| Figma | | | ☐ | |
| Password manager (shared vault) | | | ☐ | Holds all shared credentials |

Rules:
- Shared credentials live in the password manager, never in chat or the repo.
- Review the list quarterly and on every staff change ([Runbook §11](./08-DEPLOYMENT-RUNBOOK.md)).
- Editors and admins of the site itself are listed in the `editors` table (v1.1) and reviewed with the same cadence.

### Who does what in operation
| Task | Owner | Backup |
|---|---|---|
| Site health, deploys, incidents | DevOps | Frontend lead |
| Content changes | Marketing editor | Admin |
| Leads and privacy requests | Admin | PM |
| Budget and renewals | PM | Stakeholder |
| Legal and policy updates | Counsel via PM | |

## 2. Cost Register
Fill in the amounts from your own invoices. Prices and plan limits change, so check each provider's current terms.

| Item | Cycle | Renewal date | Cost | Owner |
|---|---|---|---|---|
| Domain(s) | Yearly | | `[ ]` | PM |
| Vercel plan | Monthly | | `[ ]` | DevOps |
| Supabase plan (production) | Monthly | | `[ ]` | DevOps |
| Supabase (staging) | Monthly | | `[ ]` | DevOps |
| Mailbox provider | Per user, monthly | | `[ ]` | PM |
| Transactional email / SMTP | Monthly | | `[ ]` | DevOps |
| Uptime monitoring | Monthly | | `[ ]` | DevOps |
| Optional: Plausible | Monthly | | `[ ]` | Marketing |
| Optional: CRM / email marketing | Monthly | | `[ ]` | Marketing |
| Design tool seats | Yearly | | `[ ]` | Designer |

Points that change the bill:
- **Bandwidth is the main variable cost.** A visitor who loads the full desktop set downloads up to about 12 MB. 10,000 such visits are about 120 GB. Compare that with the bandwidth included in your hosting plan. Mobile (≈ 5 MB) and AVIF reduce it.
- **Commercial use needs a paid hosting plan.** Free hobby tiers are usually restricted to non-commercial projects. Check the current Vercel terms before launch.
- **Free database projects may be paused when inactive.** A paused database would break the lead form. Use a paid production plan and check the provider's current policy.
- Review the register monthly against the invoices and set a spending alert where the provider offers one.

## 3. Routine Tasks
| Cadence | Task | Owner |
|---|---|---|
| Daily (first month), then weekly | Uptime alerts and error logs | DevOps |
| Weekly | Export or sync new leads to the CRM; check for privacy requests | Admin |
| Weekly | Review analytics (conversion, depth, stalls) ([Analytics §5](./12-ANALYTICS-SPEC.md)) | Marketing |
| Monthly | `npm outdated` and `npm audit`; apply updates (§4) | Frontend |
| Monthly | Check hosting and database usage against plan limits; reconcile the cost register | DevOps |
| Monthly | Confirm the retention jobs ran (`cron.job_run_details`, [Runbook §8](./08-DEPLOYMENT-RUNBOOK.md)) | DevOps |
| Quarterly | Rotate keys and salts ([Runbook §11](./08-DEPLOYMENT-RUNBOOK.md)) | DevOps |
| Quarterly | Access review (§1); remove unused editors | Admin |
| Quarterly | Restore test (§6) | DevOps |
| Quarterly | Re-run Lighthouse CI, accessibility spot check, and the device matrix when a new iOS or Android major version ships | QA |
| Quarterly | Check DMARC reports; tighten the policy when clean ([Domain §5.2](./13-DOMAIN-DNS-EMAIL.md)) | DevOps |
| Yearly | Review privacy policy, terms and processor agreements ([Privacy & Legal](./11-PRIVACY-AND-LEGAL.md)) | PM, Counsel |
| Yearly | Renew the domain and other annual items (§7) | PM |

## 4. Dependency and Security Patching
| Severity | Target time to patch |
|---|---|
| Critical (actively exploited, or exposes data) | 48 hours |
| High | 7 days |
| Medium | 30 days |
| Low | Next routine update |

Process:
1. Enable automated update PRs (Dependabot or Renovate) and security alerts on GitHub.
2. Every update goes through the normal PR checks: lint, unit, contract, build, Lighthouse CI.
3. Pin GSAP, Lenis and `supabase-js` to exact versions. Test their upgrades on a preview first, because they affect the scrub and the sign-in flow.
4. Record notable updates in the [CHANGELOG](../CHANGELOG.md).

## 5. Requests and Change Control
| Type of change | Examples | Who approves | How |
|---|---|---|---|
| Content | Overlay copy, timing, CTA | Marketing lead | Editor makes it directly ([Editor Guide](./10-CONTENT-EDITOR-GUIDE.md)) |
| Build-time content | Page title, meta description, OG image | Marketing lead | Developer redeploys ([SEO §3](./14-SEO-AND-DISCOVERABILITY.md)) |
| Assets | New video or frames | PM + Designer | Follow [Asset Pipeline §8](./07-ASSET-PIPELINE.md) with a version folder |
| Code | Features, fixes | PM | PR, review, checks, release ([Runbook §6](./08-DEPLOYMENT-RUNBOOK.md)) |
| Infrastructure | DNS, headers, keys, plans | PM + DevOps | Written request; DevOps keeps a copy of the previous values |
| Scope during the build | New scene, new page, new tracking | PM + Stakeholder | Add to the backlog; scope creep is rated High in the plan's risk register ([Plan §6](./17-IMPLEMENTATION-PLAN.md)). Large items go to v2. |

Support requests go to a single mailbox or channel. Target first reply: 1 business day. Production-down issues follow [Runbook §12](./08-DEPLOYMENT-RUNBOOK.md).

## 6. Restore Test (quarterly)
1. Restore the latest backup into a scratch Supabase project, or import a recent export. Confirm what your plan's backups cover ([Runbook §10](./08-DEPLOYMENT-RUNBOOK.md)).
2. Check that `pages`, `scenes`, `overlays` and `frame_sets` return the live `PageConfig`.
3. Check that the lead count is within a day of production.
4. Delete the scratch project and any exported personal data.
5. Record the date, the result and the time it took.

## 7. Annual Calendar
| Item | When | Reminder |
|---|---|---|
| Domain renewal | `[date]` | 60 and 14 days before |
| Hosting and database plan renewals | `[dates]` | 30 days before |
| SMTP, mailbox, monitoring renewals | `[dates]` | 30 days before |
| Privacy policy and terms review | `[month]` | 30 days before |
| Processor agreement review | `[month]` | 30 days before |
| Key and salt rotation | Quarterly | Calendar series |
| Accessibility and device re-test | Quarterly | Calendar series |

## 8. Retiring the Site
If the site is ever taken down:
1. Export what you must keep (leads, if you still have consent to hold them) and delete the rest.
2. Redirect the domain to a successor page, or keep a minimal notice page.
3. Keep the domain registered, so the name cannot be taken over.
4. Close or downgrade paid plans, remove access, and cancel processor agreements.
5. Record the date and decisions in the [CHANGELOG](../CHANGELOG.md).

---
