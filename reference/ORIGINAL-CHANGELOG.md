# CHANGELOG

# Changelog
Product releases follow [Semantic Versioning](https://semver.org). The format follows [Keep a Changelog](https://keepachangelog.com). Documentation revisions are listed separately at the bottom.

## [Unreleased]
### Added
- Nothing yet.

## [1.1.0] — planned 2026-12-10
### Added
- Invite-only editor sign-in at `/admin` with Supabase Auth magic links (FR-12)
- Role-based authorization (`editor`, `admin`) on all `/api/admin/*` routes (FR-13)
- TOTP MFA for admins (FR-14)
- Audit log of admin writes (FR-15)
- Leads viewer, CSV export and delete for admins (FR-16)
- `002_auth.sql` migration (`editors`, `audit_log`)

## [1.0.0] — planned 2026-11-27
### Added
- Scroll-driven canvas video with desktop and mobile frame sets (FR-1, FR-2, FR-6)
- Preloader with stall fallback (FR-3), overlays (FR-4), smooth scrolling (FR-5)
- Reduced-motion fallback (FR-7), lead capture (FR-8), analytics events (FR-10)
- Content API with static fallback (FR-9, see the scope note in the Implementation Plan §9.2)
- `001_init.sql` migration and seed data

---

## Documentation revisions
### Docs 1.4 — 2026-10-15
- README: added §4.2 Master Checklist, a 45-step tick-box list in the correct order with a gate per stage (email authentication moved to Week 1; TRD and schema, environment setup and video ordering added), and §4.3 Workflows (project, release, content change, frame update, launch day, privacy request, editor lifecycle, incident)

### Docs 1.3 — 2026-10-15
- Added `13-DOMAIN-DNS-EMAIL`, `14-SEO-AND-DISCOVERABILITY`, `15-LAUNCH-AND-GO-LIVE` and `16-MAINTENANCE-AND-OPERATIONS`
- Added the video production process to `07-ASSET-PIPELINE` (§10)
- Renumbered the Implementation Plan from 13 to 17 so it stays last
- README: the order-of-work table is now a 13-stage end-to-end process
- Added build-time injection of page title, description and overlay text for search and social crawlers (SEO §3); the Editor Guide now says these update at the next build
- Custom SMTP for sign-in emails added to Authentication §5; `SITE_URL` added to the environment variables; QA checks S-05 to S-09 and D-01 to D-05 added

### Docs 1.2 — 2026-10-15
- Added `07-ASSET-PIPELINE`, `08-DEPLOYMENT-RUNBOOK`, `09-QA-CHECKLIST`, `10-CONTENT-EDITOR-GUIDE`, `11-PRIVACY-AND-LEGAL`, `12-ANALYTICS-SPEC`
- Added the repo-root `README.md` (Build Guide) and this `CHANGELOG.md`
- Renumbered the Implementation Plan from 06 to 13 so it stays last
- Implementation Plan now links every document and phase to the new files
- Added the order-of-work table to the README (§4.1)

### Docs 1.1 — 2026-10-14
- Added `06-AUTHENTICATION` and folded authentication into the PRD, TRD, App Flow, UI/UX Brief, Backend Schema and Implementation Plan
- Added the coverage map to the Implementation Plan

### Docs 1.0
- Initial set: PRD, TRD, App Flow, UI/UX Design Brief, Backend Schema, Implementation Plan

---
