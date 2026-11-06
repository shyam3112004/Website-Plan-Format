# Canonical Document Index

**Exactly 35 individual Markdown documents.** Numbering is canonical. Files 17–35 use their canonical filenames even though the original source file groups some sections in a different reading order.

| No. | File | Document | Purpose |
|---:|---|---|---|
| 01 | [`01-PRD.md`](docs/01-PRD.md) | Product Requirements Document | Product goals, audience, scope, user stories and acceptance criteria. |
| 02 | [`02-TRD.md`](docs/02-TRD.md) | Technical Requirements Document | System constraints, architecture choices, requirements, performance and security. |
| 03 | [`03-APP-FLOW.md`](docs/03-APP-FLOW.md) | Application Flow | Site map, user journeys, page flows, states and edge cases. |
| 04 | [`04-UI-UX-DESIGN-BRIEF.md`](docs/04-UI-UX-DESIGN-BRIEF.md) | UI/UX Design Brief | Brand direction, design principles, layouts, motion, components and screens. |
| 05 | [`05-BACKEND-SCHEMA.md`](docs/05-BACKEND-SCHEMA.md) | Backend Schema | Data entities, relationships, validation, access and retention; defer if no backend. |
| 06 | [`06-AUTHENTICATION.md`](docs/06-AUTHENTICATION.md) | Authentication | Sign-in, sessions, MFA and account recovery; defer if no accounts. |
| 07 | [`07-ASSET-PIPELINE.md`](docs/07-ASSET-PIPELINE.md) | Asset Pipeline | Images, video, frames, optimization, manifests, rights and delivery. |
| 08 | [`08-DEPLOYMENT-RUNBOOK.md`](docs/08-DEPLOYMENT-RUNBOOK.md) | Deployment Runbook | Environments, hosting, build, release, rollback and headers. |
| 09 | [`09-QA-CHECKLIST.md`](docs/09-QA-CHECKLIST.md) | QA Checklist | Functional, visual, responsive, accessibility, security and release tests. |
| 10 | [`10-CONTENT-EDITOR-GUIDE.md`](docs/10-CONTENT-EDITOR-GUIDE.md) | Content Editor Guide | How authorized owners edit, review and publish website content. |
| 11 | [`11-PRIVACY-AND-LEGAL.md`](docs/11-PRIVACY-AND-LEGAL.md) | Privacy & Legal | Data inventory, consent, retention, notices and legal-review items. |
| 12 | [`12-ANALYTICS-SPEC.md`](docs/12-ANALYTICS-SPEC.md) | Analytics Specification | Measurement goals, event taxonomy, privacy and reporting. |
| 13 | [`13-DOMAIN-DNS-EMAIL.md`](docs/13-DOMAIN-DNS-EMAIL.md) | Domain, DNS & Email | Domain ownership, DNS, email routing and authentication records. |
| 14 | [`14-SEO-AND-DISCOVERABILITY.md`](docs/14-SEO-AND-DISCOVERABILITY.md) | SEO & Discoverability | Search metadata, indexing, sitemap, structured data and local discovery. |
| 15 | [`15-LAUNCH-AND-GO-LIVE.md`](docs/15-LAUNCH-AND-GO-LIVE.md) | Launch & Go-Live | Release readiness, cutover, smoke tests and launch communications. |
| 16 | [`16-MAINTENANCE-AND-OPERATIONS.md`](docs/16-MAINTENANCE-AND-OPERATIONS.md) | Maintenance & Operations | Ownership, routine updates, access, renewals, costs and incident response. |
| 17 | [`17-IMPLEMENTATION-PLAN.md`](docs/17-IMPLEMENTATION-PLAN.md) | Implementation Plan | Phases, dependencies, owners, milestones, gates, risks and coverage map. |
| 18 | [`18-FRONTEND-ARCHITECTURE.md`](docs/18-FRONTEND-ARCHITECTURE.md) | Frontend Architecture | Rendering, routing, modules, components and client-side architecture. |
| 19 | [`19-BACKEND-ARCHITECTURE.md`](docs/19-BACKEND-ARCHITECTURE.md) | Backend Architecture | Services, APIs, jobs, integrations and server architecture; defer if static. |
| 20 | [`20-COMPONENT-LIBRARY-DESIGN-SYSTEM.md`](docs/20-COMPONENT-LIBRARY-DESIGN-SYSTEM.md) | Component Library & Design System | Design tokens, typography, components, variants and usage rules. |
| 21 | [`21-STATE-MANAGEMENT.md`](docs/21-STATE-MANAGEMENT.md) | State Management | Local, URL, server and persisted state; avoid needless global state. |
| 22 | [`22-API-DOCUMENTATION.md`](docs/22-API-DOCUMENTATION.md) | API Documentation | Endpoints, payloads, auth, errors, limits and versioning; N/A if no APIs. |
| 23 | [`23-INTEGRATION-DOCUMENTATION.md`](docs/23-INTEGRATION-DOCUMENTATION.md) | Integration Documentation | Third-party services, data flows, webhooks and failure handling. |
| 24 | [`24-FILE-MEDIA-MANAGEMENT.md`](docs/24-FILE-MEDIA-MANAGEMENT.md) | File & Media Management | Storage, naming, formats, transformations, access and retention. |
| 25 | [`25-PAYMENT-SUBSCRIPTION-DOCUMENTATION.md`](docs/25-PAYMENT-SUBSCRIPTION-DOCUMENTATION.md) | Payment & Subscription Documentation | Checkout, billing, invoices and subscription lifecycle; N/A without online payments. |
| 26 | [`26-ACCESSIBILITY-STANDARD.md`](docs/26-ACCESSIBILITY-STANDARD.md) | Accessibility Standard | Keyboard, semantics, contrast, screen readers, motion and testing. |
| 27 | [`27-PERFORMANCE-ENGINEERING.md`](docs/27-PERFORMANCE-ENGINEERING.md) | Performance Engineering | Budgets, loading, rendering, caching, measurement and regression controls. |
| 28 | [`28-DISASTER-RECOVERY-BACKUP-PLAN.md`](docs/28-DISASTER-RECOVERY-BACKUP-PLAN.md) | Disaster Recovery & Backup Plan | Backups, recovery objectives, restore tests and incident recovery. |
| 29 | [`29-LOGGING-MONITORING-OBSERVABILITY.md`](docs/29-LOGGING-MONITORING-OBSERVABILITY.md) | Logging, Monitoring & Observability | Errors, uptime, signals, alerts, dashboards and privacy. |
| 30 | [`30-GIT-VERSION-CONTROL-WORKFLOW.md`](docs/30-GIT-VERSION-CONTROL-WORKFLOW.md) | Git & Version Control Workflow | Branches, commits, reviews, releases and repository hygiene. |
| 31 | [`31-ENVIRONMENT-CONFIGURATION.md`](docs/31-ENVIRONMENT-CONFIGURATION.md) | Environment Configuration | Environments, configuration, secrets and safe deployment settings. |
| 32 | [`32-USER-ROLE-PERMISSION-MANAGEMENT.md`](docs/32-USER-ROLE-PERMISSION-MANAGEMENT.md) | User, Role & Permission Management | Roles, permissions, authorization and access reviews; N/A without users. |
| 33 | [`33-BROWSER-DEVICE-COMPATIBILITY.md`](docs/33-BROWSER-DEVICE-COMPATIBILITY.md) | Browser & Device Compatibility | Supported browsers, viewport matrix and device verification. |
| 34 | [`34-DATA-MIGRATION-PLAN.md`](docs/34-DATA-MIGRATION-PLAN.md) | Data Migration Plan | Migration planning, validation, rollback and cutover; N/A without data stores. |
| 35 | [`35-USER-GUIDE-FAQ-TROUBLESHOOTING.md`](docs/35-USER-GUIDE-FAQ-TROUBLESHOOTING.md) | User Guide, FAQ & Troubleshooting | Visitor, customer and site-owner guides, FAQs and issue resolution. |

## Reading guidance

Start with 01–04 to define what is being built and how it should feel. Read 02, 18–24 for architecture and implementation. Read 07, 24 and 27 for assets and performance. Read 08–16 and 26–35 for quality, security, SEO, launch and operations. Use 17 as the master delivery schedule and cross-document coverage map. The order of work may differ from the numbering.
