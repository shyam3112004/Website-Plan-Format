<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 08 — Deployment Runbook

# Deployment Runbook
**Related:** [TRD](./02-TRD.md) §8–9, §12 · [Backend Schema](./05-BACKEND-SCHEMA.md) · [Authentication](./06-AUTHENTICATION.md) §5–6 · [Asset Pipeline](./07-ASSET-PIPELINE.md)

## 1. Environments
| Environment | Front end + API | Database | Purpose |
|---|---|---|---|
| Local | `vercel dev` | A staging Supabase project | Development |
| Preview | Vercel preview (one per PR) | Staging Supabase project | Review and QA |
| Production | Vercel production (`main`) | Production Supabase project | Live site |

Use **two Supabase projects** (staging and production) so test leads and test editors never reach production.

## 2. One-Time Setup

### 2.1 Supabase (create twice: staging, production)
1. Create the project. Pick a region close to the audience (EU for EU audiences, see [Privacy & Legal](./11-PRIVACY-AND-LEGAL.md)).
2. Link and apply migrations in order:
```bash
supabase link --project-ref <ref>
supabase db push          # applies 001_init.sql, then 002_auth.sql
psql "$DATABASE_URL" -f supabase/seed.sql      # staging only; production gets real content
```
3. Create the public Storage bucket `posters`.
4. Enable the `pg_cron` extension (Database → Extensions) for §8.
5. Configure Auth exactly as in [Authentication §5](./06-AUTHENTICATION.md). Seed the first admin with the SQL in [Backend Schema §2](./05-BACKEND-SCHEMA.md).

### 2.2 Vercel
1. Import the GitHub repo. Framework preset: **Vite**. Build command `npm run export:content && npm run build`, output `dist`. The export needs `SUPABASE_URL` and `SUPABASE_SERVICE_ROLE_KEY` at build time ([SEO §3](./14-SEO-AND-DISCOVERABILITY.md)). If you want deploys to continue during a database outage, commit the last good `public/content/home.json` and let the export step fall back to it.
2. Add the environment variables from §5 for each environment.
3. Add the production domain, then set the Supabase redirect allow-list to `https://<domain>/admin` (and the preview pattern).

### 2.3 Domain and DNS
Full process, record table and email authentication: [Domain, DNS & Email](./13-DOMAIN-DNS-EMAIL.md).

- Point the apex and `www` records at Vercel. Redirect `www` to the apex (or the reverse), one canonical host only.
- HTTPS is automatic. Confirm HSTS is on after the first stable week.

## 3. Migration Order
| Step | File | Environment |
|---|---|---|
| 1 | `001_init.sql` | Staging, then production |
| 2 | `002_auth.sql` | Staging, then production (v1.1) |
| 3 | `seed.sql` | Staging only |

Rules:
- Apply migrations **before** deploying the code that needs them. Make each migration backward compatible with the code that is currently live.
- Do not edit a migration that has been applied. Add a new numbered file.
- There are no automatic down migrations. A bad migration is fixed forward (§7).

## 4. `vercel.json`
```json
{
  "cleanUrls": true,
  "rewrites": [{ "source": "/admin(.*)", "destination": "/admin.html" }],
  "headers": [
    {
      "source": "/frames/(.*)",
      "headers": [{ "key": "Cache-Control", "value": "public, max-age=31536000, immutable" }]
    },
    {
      "source": "/((?!admin).*)",
      "headers": [
        { "key": "Content-Security-Policy",
          "value": "default-src 'self'; img-src 'self' data:; connect-src 'self'" },
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" }
      ]
    },
    {
      "source": "/admin(.*)",
      "headers": [
        { "key": "X-Robots-Tag", "value": "noindex, nofollow" },
        { "key": "Cache-Control", "value": "no-store" },
        { "key": "Content-Security-Policy",
          "value": "default-src 'self'; img-src 'self' data:; connect-src 'self' https://<project>.supabase.co" },
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "Referrer-Policy", "value": "no-referrer" }
      ]
    }
  ]
}
```
- The public rule **excludes** `/admin`. Two CSP headers on one response are combined and both must pass, which would block the admin's calls to Supabase.
- `/api/page/:slug` sets its own `Cache-Control` ([Backend Schema §5](./05-BACKEND-SCHEMA.md)).
- Replace `<project>` with the Supabase project ref of that environment.

## 5. Environment Variables
| Name | Local | Preview | Production | Notes |
|---|---|---|---|---|
| `SUPABASE_URL` | staging | staging | production | Server |
| `SUPABASE_SERVICE_ROLE_KEY` | staging | staging | production | Server only. Mark as sensitive in Vercel. |
| `IP_HASH_SALT` | any | random | random, different from preview | Server |
| `VITE_PAGE_SLUG` | `home` | `home` | `home` | Client |
| `SITE_URL` | `http://localhost:5173` | `https://$VERCEL_URL` | `https://<domain>` | Build time. Canonical and OG URLs ([SEO §3](./14-SEO-AND-DISCOVERABILITY.md)). |
| `VITE_SUPABASE_URL` | staging | staging | production | Client, `/admin` only |
| `VITE_SUPABASE_ANON_KEY` | staging | staging | production | Client, `/admin` only. Public by design. |

Before every release: confirm that `SUPABASE_SERVICE_ROLE_KEY` does not appear in `dist/` (`grep -r "service_role" dist/` returns nothing).

## 6. Release Procedure
For the first production launch (DNS cutover, 72-hour watch) use [Launch & Go-Live](./15-LAUNCH-AND-GO-LIVE.md). The steps below are for every release.

1. PR is green: lint, unit, contract, build, Lighthouse CI budgets.
2. Review the preview URL. Run the relevant parts of the [QA Checklist](./09-QA-CHECKLIST.md).
3. If the release needs a migration, apply it to production **first** (§3).
4. Merge to `main`. Vercel deploys to production.
5. Run the smoke test (§7).
6. Add a line to `CHANGELOG.md` and tag the release (`v1.0.0`).

## 7. Smoke Test and Rollback

### Smoke test (after every production deploy)
| # | Check | Expected |
|---|---|---|
| 1 | `GET /` | 200, public CSP header present |
| 2 | `GET /api/page/home` | 200, valid `PageConfig` |
| 3 | `GET /frames/desktop/frame_0001.webp` | 200, `immutable` cache header |
| 4 | Scroll the page to the end | Video scrubs, overlays appear, no console errors |
| 5 | Submit the form with a test email | 201, success message |
| 6 | `POST /api/events` with one `page_view` | 204 |
| 7 | Delete the test lead (Supabase dashboard, or admin once v1.1 is live) | Row gone |
| 8 | `GET /admin` (v1.1) | 200, `X-Robots-Tag: noindex`, admin CSP |
| 9 | `GET /api/admin/me` without a token (v1.1) | 401 |

### Rollback
| Problem | Action |
|---|---|
| Bad front-end or API deploy | Vercel → Deployments → promote the previous production deployment (instant rollback) |
| Bad content | Edit the rows in Supabase, or set the page `status` to `draft` to take it offline |
| Bad frames | Point `frame_sets.base_path` back to the previous version folder |
| Bad migration | Write a new forward migration. Restore from backup only for data loss. |

## 8. Scheduled Jobs
```sql
-- Retention (see Backend Schema §9). Runs at 03:00 UTC on the 1st of each month.
select cron.schedule('purge-analytics', '0 3 1 * *',
  $$delete from analytics_events where created_at < now() - interval '13 months'$$);

select cron.schedule('purge-audit-log', '15 3 1 * *',
  $$delete from audit_log where created_at < now() - interval '24 months'$$);
```
Check `cron.job_run_details` after the first run of each. Both jobs are created in staging and production.

## 9. Monitoring and Alerts
| What | How | Alert when |
|---|---|---|
| Uptime of `/` and `/api/page/home` | External uptime monitor, 1-minute interval | 2 failures in a row |
| API errors | Vercel function logs and error rate | 5xx rate above 2% for 10 minutes |
| Lead submissions | Weekly count | A week with zero leads after launch |
| Database size and connections | Supabase dashboard | 80% of plan limit |
| Performance regressions | Lighthouse CI on every PR | Any budget failure blocks the merge |

The availability target is 99.9% ([PRD §8](./01-PRD.md)).

## 10. Backups and Exports
- Confirm what backup coverage your Supabase plan includes, and write it here: `[plan] / [retention]`.
- Until that is confirmed, an admin exports leads weekly (`/api/admin/leads.csv`, or a dashboard CSV before v1.1).
- Exports contain personal data. Store them encrypted, share them only with people who need them, and delete old copies.

## 11. Secrets and Offboarding
| Event | Action |
|---|---|
| Quarterly | Rotate `SUPABASE_SERVICE_ROLE_KEY` and `IP_HASH_SALT`. Update every Vercel environment, redeploy. |
| Suspected leak | Rotate immediately. Review `audit_log` and Supabase logs for the exposure window. |
| Editor leaves | Set `disabled = true` in `editors`, delete the user in Supabase Auth, remove Vercel, GitHub and Supabase org access. |
| Developer leaves | Remove repo, Vercel and Supabase access. Rotate keys they could read. |

## 12. Incident Response
1. **Detect:** alert, or a report from the team or a visitor.
2. **Contain:** roll back (§7). For a suspected account compromise, disable the editor and rotate keys.
3. **Assess:** what data or content was affected, and for how long.
4. **Notify:** if personal data was exposed, follow the breach steps in [Privacy & Legal §8](./11-PRIVACY-AND-LEGAL.md).
5. **Fix and record:** write a short post-incident note (cause, impact, fix, follow-ups) in the repo.

---
