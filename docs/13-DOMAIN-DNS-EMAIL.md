<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 13 — Domain Dns Email

# Domain, DNS & Email
**Related:** [Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md) §2.3 · [Authentication](./06-AUTHENTICATION.md) §5 · [SEO & Discoverability](./14-SEO-AND-DISCOVERABILITY.md) · [Launch & Go-Live](./15-LAUNCH-AND-GO-LIVE.md) §4 · [Maintenance & Operations](./16-MAINTENANCE-AND-OPERATIONS.md) §1, §7

## 1. Purpose and Ownership
This document covers everything between "we need an address" and "mail and pages arrive reliably": registering the domain, DNS records, connecting Vercel, email authentication, and the email sender Supabase uses for sign-in links.

Start it in **week 1**. Registrar changes can take days, and email authentication needs a few weeks of monitoring before it can be tightened.

| Item | Owner | Where it is recorded |
|---|---|---|
| Registrar account | `[Company]` (a role mailbox, not a person's email) | Access register ([Maintenance §1](./16-MAINTENANCE-AND-OPERATIONS.md)) |
| DNS zone | DevOps | Record table in §3 |
| Mailboxes | `[Company]` admin | Access register |
| Renewal dates | PM | Calendar in [Maintenance §7](./16-MAINTENANCE-AND-OPERATIONS.md) |

## 2. Register the Domain
| Step | Detail |
|---|---|
| Choose the name | Short, easy to say, no hyphens or numbers if possible. Check trademarks and social handles before buying. |
| Choose the TLD | `.com` if available, plus the country TLD you serve. Register obvious typo variants only if they matter. |
| Pick a registrar | One with two-factor login, registrar lock and clear transfer rules. |
| Register under the company | Company name as registrant, role mailbox as contact. Never a freelancer's or employee's personal account. |
| Turn on protection | 2FA on the account, registrar lock, WHOIS privacy where the registry allows it, auto-renew on. |
| Register for several years | At least 2. Put the expiry date in the renewal calendar. |
| Record it | Registrar, account owner, expiry date and 2FA method go in the access register. |

## 3. DNS Plan
Keep DNS in **one** place. Use the registrar's DNS, Vercel DNS or a DNS provider such as Cloudflare. Mixing them is the most common cause of "it works for me" problems.

If you use a proxy (for example Cloudflare's orange cloud), set the records that point at Vercel to **DNS only**, unless you have tested the proxy with the CSP, caching and the `/frames/*` immutable headers.

| Type | Host | Value | Purpose |
|---|---|---|---|
| A | `@` | Value Vercel shows for the apex | Site (apex) |
| CNAME | `www` | Value Vercel shows | Site (`www`), redirected to the apex |
| MX | `@` | Your mail provider's MX records | Mailboxes |
| TXT | `@` | `v=spf1 include:<provider> ~all` | SPF. **One** SPF record only, see §5.2 |
| TXT or CNAME | `<selector>._domainkey` | From the mail or SMTP provider | DKIM |
| TXT | `_dmarc` | `v=DMARC1; p=none; rua=mailto:dmarc@<domain>` | DMARC, see §5.2 |
| TXT | `@` | From Search Console | Ownership check ([SEO §10](./14-SEO-AND-DISCOVERABILITY.md)) |
| CAA (optional) | `@` | `0 issue "letsencrypt.org"` | Restricts who may issue certificates. Vercel certificates come from Let's Encrypt, so leaving it out of a CAA record breaks issuance. |

Use the exact apex and `www` values shown in Vercel (Project → Settings → Domains). They can differ per project, so do not copy them from another site.

## 4. Connect the Domain to Vercel
1. Add the apex and `www` in Vercel. Choose the canonical host (apex recommended).
2. Set the other host to **redirect** to it (308). Do not serve the site on both.
3. Add the DNS records from §3. Wait until Vercel shows both as valid and the certificate as issued (HTTPS is automatic).
4. Staging: use the Vercel preview URL, or add `staging.<domain>` pointing at a staging deployment.
5. In Supabase Auth, add the production and staging URLs to the redirect allow-list ([Authentication §5](./06-AUTHENTICATION.md)).
6. Set `SITE_URL` in Vercel to the canonical URL ([SEO §3](./14-SEO-AND-DISCOVERABILITY.md)).
7. HSTS: once the site has been stable for a week and every subdomain you use is HTTPS, add the header:
```json
{ "key": "Strict-Transport-Security", "value": "max-age=31536000; includeSubDomains" }
```
Add it to the public header rule in `vercel.json`. Do not submit the domain to the HSTS preload list until you are sure you never need plain HTTP anywhere.

## 5. Email

### 5.1 Mailboxes
Create role mailboxes on the domain, not personal ones:

| Address | Used for |
|---|---|
| `hello@` | Public contact |
| `privacy@` | Privacy and deletion requests ([Privacy & Legal §4](./11-PRIVACY-AND-LEGAL.md)) |
| `dmarc@` | DMARC reports (§5.2) |
| `no-reply@` (or `auth@`) | Sender for sign-in emails (§5.3), usually without a mailbox |

Use a mailbox provider (Google Workspace, Microsoft 365, Zoho or similar). Add its MX records.

### 5.2 SPF, DKIM, DMARC
| Record | What it does | Rule |
|---|---|---|
| SPF | Lists who may send for the domain | Exactly one `v=spf1` record. Merge providers into it: `v=spf1 include:<mailbox provider> include:<smtp provider> ~all` |
| DKIM | Signs each message | Add the selector records each provider gives you |
| DMARC | Tells receivers what to do when SPF or DKIM fail | Roll out in stages |

DMARC rollout:
1. `p=none` with reports to `dmarc@`. Run for 2 to 4 weeks and fix any legitimate sender that fails.
2. `p=quarantine` (start with `pct=25`, then 100).
3. `p=reject` once reports are clean.

### 5.3 Sign-in Emails (Supabase Auth, v1.1)
The built-in Supabase sender is intended for testing, is heavily rate-limited, and may only deliver to your project's team members. Check the current Supabase documentation. Do not invite real editors until you have configured your own sender.

1. Pick a transactional email provider (for example Postmark, Resend, SendGrid or Amazon SES). Verify the sending domain there and add its SPF and DKIM records (§5.2).
2. In Supabase → Authentication → SMTP settings: enter host, port, username, password, and sender `no-reply@<domain>` with a clear sender name.
3. Edit the magic-link and invite templates: plain wording, the company name, and no promotional content.
4. Raise the auth email rate limit only as far as you need, after the custom sender works.
5. Test with a real inbox on each major provider you expect editors to use. In Gmail, "Show original" must show `SPF: PASS`, `DKIM: PASS`, `DMARC: PASS`.
6. Add the provider to the access register and the cost register ([Maintenance §1–2](./16-MAINTENANCE-AND-OPERATIONS.md)).

## 6. Other Verifications
Search Console and Bing Webmaster use a DNS TXT record to prove ownership. Add them after the site is live ([SEO §10](./14-SEO-AND-DISCOVERABILITY.md)). Keep the records in place permanently, since removing them revokes access.

## 7. TTL and Cutover
- Normal TTL: 3600 s.
- One to two days before the cutover, lower the TTL on the records you will change to 300 s ([Launch & Go-Live §4](./15-LAUNCH-AND-GO-LIVE.md)).
- After the site has been stable for a week, raise it back.

## 8. Verification Commands
```bash
dig +short A example.com                 # points at Vercel?
dig +short CNAME www.example.com
dig +short MX example.com
dig +short TXT example.com               # exactly one v=spf1 record?
dig +short TXT _dmarc.example.com
dig +short TXT <selector>._domainkey.example.com
curl -sI https://example.com | head -n 20           # 200, security headers
curl -sI http://www.example.com | grep -i location  # 308 to the canonical host
```
Also check the certificate and headers with a public SSL test and a security-headers scanner.

## 9. Renewals and Risks
| Risk | Impact | Prevention |
|---|---|---|
| Domain expires | Site and email go offline; the name can be taken | Auto-renew, 2+ year term, two calendar reminders (60 and 14 days) |
| Registrar account lost or hijacked | Loss of the domain | Company-owned account, 2FA, registrar lock, two named admins |
| Payment card expires | Auto-renew fails | Use a company card with a shared billing mailbox |
| Second SPF record added | Mail starts failing SPF | Merge into one record (§5.2) |
| DNS changed by accident | Outage | Change DNS only through the change process ([Maintenance §5](./16-MAINTENANCE-AND-OPERATIONS.md)); keep a copy of the record table |

## 10. Troubleshooting
| Symptom | Likely cause | Fix |
|---|---|---|
| Vercel says "Invalid configuration" | Records not saved or not propagated, or wrong values | Re-check §3 against the values in Vercel; wait up to the TTL |
| Certificate not issued | CAA record excludes Let's Encrypt, or the DNS still points elsewhere | Fix the CAA record or the A/CNAME |
| Site loads on `www` but not the apex (or reverse) | One record missing | Add the missing record and confirm the redirect |
| Sign-in emails land in spam | SPF/DKIM/DMARC not aligned for the sender domain | Verify the sending domain at the provider; check headers (§5.3) |
| Sign-in emails never arrive | Using the built-in Supabase sender, or rate limits | Configure custom SMTP (§5.3) |
| Mail from the mailbox fails SPF | Two SPF records, or a provider missing from the record | Merge into one |

---
