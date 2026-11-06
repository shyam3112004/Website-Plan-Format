<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 11 — Privacy And Legal

# Privacy & Legal
> **This is a working template, not legal advice.** Have a qualified lawyer review the consent text, policies and retention rules before launch, in each region you serve.

**Related:** [PRD](./01-PRD.md) §8 · [Backend Schema](./05-BACKEND-SCHEMA.md) §9 · [Analytics Spec](./12-ANALYTICS-SPEC.md) · [Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md) §10–12

## 1. Roles
| Party | Role |
|---|---|
| `[Company legal name]` | Controller of visitor and lead data. Contact: `[privacy@company.com]` |
| Vercel | Processor: hosting, serverless functions, logs |
| Supabase | Processor: database, authentication, storage |
| CRM or email tool (if added, see Plan §7) | Processor, once leads are exported to it |
| Email provider (mailboxes and sign-in emails) | Processor: holds editor emails, sends sign-in links, receives privacy requests |
| Plausible (optional) | Processor for cookieless analytics |

Sign a data processing agreement with each processor. Choose hosting regions to match your audience.

## 2. Data Inventory
| Data | Where | Why | Retention |
|---|---|---|---|
| Lead email, name | `leads` | Follow up on interest | Until deletion or unsubscribe request |
| Consent timestamp (`consent_at`) | `leads` | Proof of consent | Same as the lead |
| UTM source and campaign | `leads` | Measure campaigns | Same as the lead |
| IP hash, user agent | `leads` | Spam control. The salted hash is still personal data. | Same as the lead |
| Session id, event name, device, event props | `analytics_events` | Understand usage. Contains no name or email. | 13 months |
| Editor email, role | `editors`, Supabase Auth | Access control | While access is needed |
| Admin actions, IP hash | `audit_log` | Accountability | 24 months |

Do not add new fields without updating this table and the privacy policy.

## 3. Consent (Lead Form)
- The checkbox is **unchecked by default** and required. It is not bundled with anything else.
- Suggested text (adapt to your purpose):

  > ☐ I agree to receive launch updates from **[Company]**. I can unsubscribe at any time. See our [Privacy Policy](/privacy).

- The server refuses a submission without `consent: true` and stores the time in `consent_at`.
- The text next to the checkbox must say what the email will be used for. If the use changes (for example newsletters), collect new consent.

## 4. Retention and Deletion
| Request | How it is handled |
|---|---|
| Delete my data | An admin deletes the lead (`DELETE /api/admin/leads/:id`, or the dashboard before v1.1) |
| Unsubscribe | Reply to the request and delete the lead. If the lead was exported to a CRM, remove or suppress it there too. |
| Access or copy of my data | Admin exports the single lead and sends it to the verified requester |
| Correct my data | Admin edits the lead in the dashboard |

Process:
1. Requests arrive at `[privacy@company.com]`.
2. Verify the requester controls the email address.
3. Complete the request within 30 days and confirm it in writing.
4. Record the request and the date (a simple log is enough; do not store the lead data in it).

## 5. Cookies and Browser Storage
| Item | Purpose | Notes |
|---|---|---|
| `sessionStorage` `sid` | Random session id for analytics | Cleared when the tab closes. Not shared across sites. |
| `localStorage` (Supabase session) | Keeps editors signed in at `/admin` | Admin area only; strictly necessary |

No advertising or tracking cookies are used. With this setup a cookie banner is probably not required for visitors, but confirm with counsel for each region. Adding any third-party script or cookie changes this answer.

## 6. Privacy Policy Outline (`/privacy`)
1. Who we are and how to contact us
2. What we collect (table from §2, in plain language)
3. Why we use it and the legal basis (consent for leads; legitimate interest for security and analytics, if your counsel agrees)
4. Who receives it (processors in §1) and where it is stored
5. How long we keep it
6. Your rights: access, correction, deletion, portability, objection, withdrawing consent, complaint to the regulator
7. Cookies and storage (§5)
8. Children: the site is not directed at children `[confirm age limit]`
9. Changes to the policy and the last-updated date

## 7. Terms Outline (`/terms`)
1. Acceptance and eligibility
2. Use of the site and acceptable use
3. Intellectual property (video, text, brand)
4. Disclaimers and limitation of liability
5. Links to third parties
6. Governing law and contact `[jurisdiction]`
7. Changes and the last-updated date

## 8. Data Breach Response
1. Contain the problem ([Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md) §12).
2. Record what happened, which data, how many people, and when it was found.
3. Where the GDPR applies, report a breach that risks people's rights to the supervisory authority **within 72 hours** of becoming aware. Tell affected people without undue delay when the risk is high.
4. Keep the record even when no report is required.

## 9. Pre-Launch Legal Checklist
- [ ] `/privacy` and `/terms` written, reviewed by counsel and linked in the footer
- [ ] Consent text approved and matching the form
- [ ] Processor agreements signed (Vercel, Supabase, any CRM)
- [ ] Hosting regions confirmed
- [ ] Deletion process and `[privacy@company.com]` mailbox in place
- [ ] Data inventory (§2) matches what the code stores
- [ ] Retention jobs created ([Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md) §8)
- [ ] Cookie and storage review done for every region served

---
