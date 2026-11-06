# Modern Guideways — Hermes Agent Skills & Document Map

**Document type:** Separate skills companion for the 35-document website plan  
**Version:** 1.1  
**Last checked:** 2026-11-04  
**Purpose:** Tell Hermes which specialist Agent Skills to load while working through each document. This file supplements the 35 Markdown documents; it does not replace or rewrite them.

---

## 1. Project scope this skills map must respect

Modern Guideways needs a professional, searchable company website, not an online store or web application.

### Minimum launch scope

- Company profile, About, product categories, manufacturing/capability information, gallery and contact details.
- Existing generated video frames are already available. Audit and integrate them; do not regenerate or re-split the source video unless the owner approves it.
- Use the established metallic-blue, black, white and steel-grey brand direction.
- Phone, WhatsApp, email and directions are the enquiry routes.
- Search-engine discoverability, technical SEO, responsive design, accessibility and fast static hosting are important.
- No online payment, checkout, subscriptions, customer accounts, paid CMS, paid AI APIs or database are required for the minimum release.
- Prefer a static-site architecture and a free hosting tier. Confirm current vendor pricing, limits and commercial terms before deployment.

### Important distinction

The 35-document pack includes architecture templates for backend, authentication, APIs, payments, roles and migrations because it follows the supplied comprehensive documentation format. Their presence does **not** authorize Hermes to build those features. For the minimum launch, treat docs 05, 06, 19, 21 (server-side state), 22, 23, 25, 28 (database recovery), 31 (secrets not used by a static site), 32 and 34 as **deferred, not applicable, or contingency documentation** unless the business later approves a real requirement.

The implementation plan and PRD are the scope authorities. When they conflict with the latest owner instruction, record the conflict and follow the latest owner-approved scope.

---

## 2. Core skill library

These are the recommended skills for the project. A skill is a reusable instruction/workflow for Hermes; it is not a separate AI model, paid API or website dependency. Installing a skill does not itself incur a subscription charge, but any external AI model/API used by Hermes may have its own usage cost.

| Skill | Best use in this project | Priority |
|---|---|---|
| `design-taste-frontend` — Taste Skill | Anti-template art direction; audit an existing page; create a distinctive visual concept; inspect screenshots and iterate | Essential for redesign |
| `frontend-design` — Anthropic | Design direction, deliberate typography, palette, layout, content hierarchy and non-generic UI | Essential for redesign |
| `frontend-ui-engineering` — Addy Osmani | Responsive production UI, component architecture, design systems, accessibility and implementation quality | Essential for build |
| `emil-design-eng` — Emil Kowalski | High-quality motion, micro-interactions, transitions, hover/focus states and animation restraint | Recommended |
| `web-design-guidelines` — Vercel | Final UI review for accessibility, interaction, typography, forms, images and performance | Essential for QA |
| `source-driven-development` — Addy Osmani | Verify framework/provider behavior against current official docs before implementation | Essential for technical decisions |
| `spec-driven-development` — Addy Osmani | Convert confirmed business requirements into scope and acceptance criteria | Recommended |
| `planning-and-task-breakdown` — Addy Osmani | Turn the implementation plan into small, ordered work items | Recommended |
| `incremental-implementation` — Addy Osmani | Implement and verify in small vertical slices rather than generating the whole site at once | Essential for build |
| `browser-testing-with-devtools` — Addy Osmani | Test actual rendered behavior in a browser and inspect layout/runtime problems | Essential for QA |
| `test-driven-development` — Addy Osmani | Add tests for deterministic behavior such as frame mapping, navigation and utilities | Recommended |
| `performance-optimization` — Addy Osmani | Measure and improve frame payload, rendering, loading and runtime performance | Recommended |
| `security-and-hardening` — Addy Osmani | Check external links, form handling, headers, dependencies and secrets | Recommended |
| `documentation-and-adrs` — Addy Osmani | Keep technical decisions, setup and handover docs consistent | Recommended |
| `writing-guidelines` — Vercel | Review the clarity and consistency of the website's copy and documentation | Optional |
| `git-workflow-and-versioning` — Addy Osmani | Make safe commits, branches and versioned releases | Recommended |
| `shipping-and-launch` — Addy Osmani | Release checklist, launch readiness and rollback thinking | Recommended |
| `observability-and-instrumentation` — Addy Osmani | Add only lightweight, privacy-aware monitoring if it is actually needed | Optional |
| `debugging-and-error-recovery` — Addy Osmani | Reproduce, isolate and fix browser/build/deployment faults | Recommended |
| `code-review-and-quality` — Addy Osmani | Final code review and maintainability checks | Recommended |

**Do not install every skill just because it is listed.** Start with the essential skills. Add the others when the task calls for them. Hermes loads skill instructions on demand, but a smaller working set makes the workflow easier to manage.

---

## 3. Install and manage skills in Hermes

Use Hermes' native Skills Hub commands. The hub searches supported registries, lets you inspect a skill before installation, and runs a security scan during installation.

### Step A — Search

Run these in the Hermes terminal/PowerShell session:

```powershell
hermes skills search "design taste frontend" --source skills-sh
hermes skills search "frontend design" --source skills-sh
hermes skills search "frontend ui engineering" --source skills-sh
hermes skills search "emil design engineering" --source skills-sh
hermes skills search "web design guidelines" --source skills-sh
hermes skills search "source driven development" --source skills-sh
hermes skills search "browser testing with devtools" --source skills-sh
```

### Step B — Inspect before installing

Copy the exact source-qualified identifier shown by Hermes search. Then run:

```powershell
hermes skills inspect <EXACT-SOURCE-QUALIFIED-SKILL-ID>
```

Read the skill's description, source, requested tools/access and security result. Do not install a similarly named skill from an unknown publisher merely because the name matches.

### Step C — Install

Use the exact identifier returned by Hermes:

```powershell
hermes skills install <EXACT-SOURCE-QUALIFIED-SKILL-ID>
```

Repeat for the selected skills. Do not add `--force` to bypass a security warning. A dangerous verdict must not be overridden.

### Step D — Verify and audit

```powershell
hermes skills list
hermes skills audit
hermes skills check
```

In Hermes chat, installed skills can be invoked by slash command. For example:

```text
/design-taste-frontend Audit and redesign the existing Modern Guideways homepage.
/frontend-design Create a design direction using the approved industrial brand palette.
/frontend-ui-engineering Implement the approved responsive interface.
/emil-design-eng Refine the scroll animation and micro-interactions.
/web-design-guidelines Review the finished pages for UI quality and accessibility.
```

If a particular skill does not appear in Hermes search, use its verified official repository to confirm the current install route. Do not guess a source-qualified identifier.

### Official references

- Hermes Skills system and source-qualified install workflow: https://hermes-agent.nousresearch.com/docs/user-guide/features/skills
- Hermes CLI commands: https://hermes-agent.nousresearch.com/docs/reference/cli-commands
- Taste Skill: https://github.com/Leonxlnx/taste-skill
- Anthropic Frontend Design: https://github.com/anthropics/skills/tree/main/skills/frontend-design
- Emil Design Engineering: https://github.com/emilkowalski/skills
- Vercel Agent Skills: https://github.com/vercel-labs/agent-skills
- Addy Osmani Agent Skills: https://github.com/addyosmani/agent-skills

---

## 4. Skill assignment for each document

**How to read this table:** Load the first skill listed as the primary workflow, then add only the supporting skills needed for the current task. A document's assigned skill is a helper, not a reason to build a feature outside the minimum launch scope.

| Doc | Document | Primary skills | Supporting skills | Minimum-launch status |
|---:|---|---|---|---|
| 01 | PRD | `interview-me`, `spec-driven-development` | `constraint-driven-development`, `documentation-and-adrs` | Active — confirm scope and acceptance criteria |
| 02 | TRD | `source-driven-development`, `spec-driven-development` | `planning-and-task-breakdown`, `frontend-ui-engineering` | Active — static architecture only |
| 03 | App Flow | `frontend-design`, `frontend-ui-engineering` | `browser-testing-with-devtools`, `documentation-and-adrs` | Active — visitor, contact and reduced-motion flows |
| 04 | UI/UX Design Brief | `design-taste-frontend`, `frontend-design` | `frontend-ui-engineering`, `emil-design-eng` | Active — key design authority |
| 05 | Backend Schema | `api-and-interface-design`, `source-driven-development` | `security-and-hardening`, `documentation-and-adrs` | Deferred — no database in simple static launch |
| 06 | Authentication | `security-and-hardening`, `source-driven-development` | `test-driven-development` | Not applicable — no visitor/admin accounts in v1 |
| 07 | Asset Pipeline | `performance-optimization`, `frontend-ui-engineering` | `browser-testing-with-devtools`, `source-driven-development` | Active — audit existing frames; no re-extraction by default |
| 08 | Deployment Runbook | `source-driven-development`, `shipping-and-launch` | `security-and-hardening`, `documentation-and-adrs` | Active — use chosen free static host; verify current limits |
| 09 | QA Checklist | `browser-testing-with-devtools`, `test-driven-development` | `web-design-guidelines`, `performance-optimization`, `code-review-and-quality` | Active — release gate |
| 10 | Content Editor Guide | `documentation-and-adrs`, `writing-guidelines` | `frontend-ui-engineering` | Active — owner edits approved static content in source files |
| 11 | Privacy & Legal | `source-driven-development`, `security-and-hardening` | `documentation-and-adrs` | Active — minimal data collection; human review required for legal text |
| 12 | Analytics Spec | `source-driven-development`, `observability-and-instrumentation` | `security-and-hardening`, `documentation-and-adrs` | Optional — use privacy-aware free analytics only if approved |
| 13 | Domain, DNS & Email | `source-driven-development`, `shipping-and-launch` | `security-and-hardening`, `documentation-and-adrs` | Active — domain is the planned paid item; verify existing domain ownership first |
| 14 | SEO & Discoverability | `source-driven-development`, `writing-guidelines` | `frontend-ui-engineering`, `browser-testing-with-devtools` | Active — Google indexing, metadata, sitemap, Search Console, local business details |
| 15 | Launch & Go-Live | `shipping-and-launch`, `browser-testing-with-devtools` | `performance-optimization`, `documentation-and-adrs` | Active — launch checklist and smoke tests |
| 16 | Maintenance & Operations | `debugging-and-error-recovery`, `documentation-and-adrs` | `security-and-hardening`, `performance-optimization` | Active — lightweight static-site maintenance |
| 17 | Frontend Architecture (`18-FRONTEND-ARCHITECTURE.md`) | `spec-driven-development`, `incremental-implementation` | `frontend-ui-engineering`, `source-driven-development` | Active — primary code architecture |
| 18 | Backend Architecture (`19-BACKEND-ARCHITECTURE.md`) | `api-and-interface-design`, `security-and-hardening` | `source-driven-development`, `documentation-and-adrs` | Deferred — keep as future reference only |
| 19 | Component Library & Design System (`20-COMPONENT-LIBRARY-DESIGN-SYSTEM.md`) | `design-taste-frontend`, `frontend-design` | `frontend-ui-engineering`, `web-design-guidelines` | Active — metallic blue / black / white / steel-grey tokens |
| 20 | State Management (`21-STATE-MANAGEMENT.md`) | `frontend-ui-engineering`, `code-simplification` | `incremental-implementation`, `test-driven-development` | Minimal — simple local UI state only; no global state library by default |
| 21 | API Documentation (`22-API-DOCUMENTATION.md`) | `api-and-interface-design`, `source-driven-development` | `security-and-hardening`, `test-driven-development` | Not applicable — no custom API required for static contact links |
| 22 | Integration Documentation (`23-INTEGRATION-DOCUMENTATION.md`) | `source-driven-development`, `security-and-hardening` | `api-and-interface-design`, `documentation-and-adrs` | Minimal — domain, hosting, Search Console and contact links only |
| 23 | File & Media Management (`24-FILE-MEDIA-MANAGEMENT.md`) | `performance-optimization`, `frontend-ui-engineering` | `source-driven-development`, `browser-testing-with-devtools` | Active — product photography, logos and existing frame files |
| 24 | Payment & Subscription (`25-PAYMENT-SUBSCRIPTION-DOCUMENTATION.md`) | `spec-driven-development`, `documentation-and-adrs` | None | Not applicable — explicitly no website payment or subscription features |
| 25 | Accessibility (`26-ACCESSIBILITY-STANDARD.md`) | `web-design-guidelines`, `frontend-ui-engineering` | `browser-testing-with-devtools`, `test-driven-development` | Active — keyboard, contrast, alt text and reduced motion |
| 26 | Performance Engineering (`27-PERFORMANCE-ENGINEERING.md`) | `performance-optimization`, `browser-testing-with-devtools` | `frontend-ui-engineering`, `web-design-guidelines` | Active — image sequence, mobile payload and scroll performance |
| 27 | Disaster Recovery & Backup (`28-DISASTER-RECOVERY-BACKUP-PLAN.md`) | `documentation-and-adrs`, `shipping-and-launch` | `git-workflow-and-versioning`, `security-and-hardening` | Minimal — Git history and recoverable static assets; no database |
| 28 | Logging, Monitoring & Observability (`29-LOGGING-MONITORING-OBSERVABILITY.md`) | `observability-and-instrumentation`, `source-driven-development` | `performance-optimization`, `security-and-hardening` | Optional — uptime/indexing checks; avoid unnecessary tracking |
| 29 | Git & Version Control (`30-GIT-VERSION-CONTROL-WORKFLOW.md`) | `git-workflow-and-versioning` | `code-review-and-quality`, `documentation-and-adrs` | Active — protect source and history |
| 30 | Environment Configuration (`31-ENVIRONMENT-CONFIGURATION.md`) | `source-driven-development`, `security-and-hardening` | `documentation-and-adrs` | Minimal — do not create secret variables unless a real service needs them |
| 31 | User, Role & Permission Management (`32-USER-ROLE-PERMISSION-MANAGEMENT.md`) | `security-and-hardening`, `spec-driven-development` | `test-driven-development` | Not applicable — no website accounts or admin dashboard in v1 |
| 32 | Browser & Device Compatibility (`33-BROWSER-DEVICE-COMPATIBILITY.md`) | `browser-testing-with-devtools`, `frontend-ui-engineering` | `web-design-guidelines`, `performance-optimization` | Active — desktop, tablet, mobile and reduced-motion checks |
| 33 | Data Migration (`34-DATA-MIGRATION-PLAN.md`) | `deprecation-and-migration`, `source-driven-development` | `test-driven-development`, `documentation-and-adrs` | Not applicable — no database migration for the static v1 |
| 34 | User Guide / FAQ / Troubleshooting (`35-USER-GUIDE-FAQ-TROUBLESHOOTING.md`) | `documentation-and-adrs`, `writing-guidelines` | `debugging-and-error-recovery`, `browser-testing-with-devtools` | Active — visitor contact/use guide and site-owner update guide |
| 35 | Implementation Plan (`17-IMPLEMENTATION-PLAN.md`) | `planning-and-task-breakdown`, `spec-driven-development` | `incremental-implementation`, `shipping-and-launch` | Active — schedule the minimum static website first |

---

## 5. Recommended installation set for this project

For the first redesign/build cycle, install only these first:

1. `design-taste-frontend`
2. `frontend-design`
3. `frontend-ui-engineering`
4. `emil-design-eng`
5. `web-design-guidelines`
6. `browser-testing-with-devtools`
7. `source-driven-development`
8. `incremental-implementation`
9. `performance-optimization`
10. `shipping-and-launch`

Add `documentation-and-adrs`, `writing-guidelines`, `security-and-hardening`, `git-workflow-and-versioning` and `debugging-and-error-recovery` when you begin those specific workstreams. Do not install backend/payment skills just to support features that are out of scope.

### Suggested sequence when using Hermes

**Design stage**

```text
/design-taste-frontend Audit the existing Modern Guideways site in the browser. Keep the approved metallic-blue, black, white and steel-grey brand direction. Produce a distinctive design plan before changing code.
/frontend-design Turn the approved plan into page layouts, typography and design tokens.
```

**Build stage**

```text
/frontend-ui-engineering Implement the approved static website in small tested steps. Preserve the existing Vite/Vanilla JS structure and image frames.
/emil-design-eng Add restrained, purposeful GSAP motion and interaction polish. Do not add animation just for decoration.
```

**Verification stage**

```text
/browser-testing-with-devtools Test the actual website at desktop, tablet and mobile widths. Report screenshots and reproducible defects.
/web-design-guidelines Audit accessibility, interaction, responsive behavior, typography and performance. Fix the findings and retest.
```

---

## 6. Agent work boundaries

- Read the relevant project document before coding; do not infer requirements from a filename alone.
- Inspect the current repository before editing. Preserve useful existing work and the existing frame sequence.
- Ask for approval before adding paid services, new API providers, a database, account system, payment feature or non-free hosting plan.
- Never place secrets or private API keys in frontend code, Git or documentation.
- Do not invent product specifications, certifications, customer names, warranty promises or company claims.
- For every implementation task, state the scope, files to change, acceptance checks and how the result will be verified.
- Capture and inspect actual browser screenshots. Do not claim visual QA passed based only on source-code inspection.
- Run the available build, lint and test commands before reporting completion. Record any command that could not be run.
- Review skill security results before installation; do not bypass a dangerous verdict.

---

## 7. Source and verification notes

Skill names and installation mechanisms change. The Hermes search/inspect/install flow in §3 is the authoritative way to resolve the current skill identifier. The skills listed in this guide are a curated recommendation set, not a guarantee that every third-party skill will be returned by every registry on every Hermes version.

Primary source documentation consulted on 2026-11-04:

- Hermes Skills System: https://hermes-agent.nousresearch.com/docs/user-guide/features/skills
- Hermes CLI reference: https://hermes-agent.nousresearch.com/docs/reference/cli-commands
- Anthropic Frontend Design skill: https://github.com/anthropics/skills/tree/main/skills/frontend-design
- Vercel Agent Skills and Web Design Guidelines: https://github.com/vercel-labs/agent-skills
- Taste Skill repository and install instructions: https://github.com/Leonxlnx/taste-skill
- Emil Design Engineering skill: https://github.com/emilkowalski/skills
- Addy Osmani Agent Skills and lifecycle catalog: https://github.com/addyosmani/agent-skills

This guide recommends skills based on their published descriptions and documented workflows. It does not claim that every skill has been independently security-audited or tested inside the user's local Hermes installation.
