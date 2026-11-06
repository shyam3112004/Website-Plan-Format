<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 01 — Prd

# Product Requirements Document (PRD)
**Product:** Scroll-Driven Video Animation Website
**Version:** 1.2 · **Status:** Draft · **Last updated:** 2026-10-15
**Related:** [TRD](./02-TRD.md) · [Authentication](./06-AUTHENTICATION.md) · [Privacy & Legal](./11-PRIVACY-AND-LEGAL.md) · [Analytics Spec](./12-ANALYTICS-SPEC.md)

## 1. Overview
A marketing landing page where scrolling controls a cinematic product video, frame by frame, in both directions. Text overlays tell the product story in sync with the video, and the page ends with a call to action (CTA) that captures leads. A small, invite-only admin area lets the team edit content and manage leads.

## 2. Problem Statement
Static landing pages don't hold attention, and autoplay videos take control away from the user. Brands want an immersive page that the user controls, like Apple's product pages, without the cost of a WebGL/3D build.

## 3. Goals
| # | Goal | Metric |
|---|---|---|
| G1 | Engage visitors with an immersive story | Avg. scroll depth ≥ 70% |
| G2 | Convert interest into leads | CTA conversion ≥ 3% |
| G3 | Fast, smooth experience | LCP < 2.5s, 60 fps scrub on mid-range devices |
| G4 | Easy content updates | Overlay copy editable without a redeploy |
| G5 | Safe content and lead management | Only invited, role-checked users can change content or read leads |

## 4. Non-Goals (v1)
- E-commerce checkout or payments
- **User accounts for visitors.** Authentication covers editors and admins only.
- WebGL/3D real-time rendering
- Multi-language support (planned for v2)

## 5. Target Users
| Persona | Description | Need |
|---|---|---|
| **Visitor** | Prospective customer, desktop or mobile | Understand the product quickly and in an engaging way |
| **Marketing Editor** | Non-technical team member | Change headlines and CTAs and swap scenes |
| **Admin** | Owner of the site and its data | Control who has access, handle leads and deletion requests |
| **Developer** | Maintains the site | Simple, modular, documented codebase |

## 6. User Stories
| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-1 | Visitor | scroll and see the video respond instantly | I feel in control of the story |
| US-2 | Visitor | scroll backwards smoothly | I can re-watch details |
| US-3 | Visitor | see a loading indicator | I know the experience is coming |
| US-4 | Visitor | read clear text at each stage | I understand the key features |
| US-5 | Visitor | leave my email at the end | I get launch updates |
| US-6 | Visitor (reduced motion) | get a static version | I avoid motion discomfort |
| US-7 | Editor | edit overlay text and timing in a CMS | I don't need a developer |
| US-8 | Editor | see lead submissions | I can follow up |
| US-9 | Editor | sign in with my email, no password | I can edit copy securely |
| US-10 | Admin | invite and disable editors | I control who can change the site |
| US-11 | Admin | view, export and delete leads | I can follow up and honor deletion requests |
| US-12 | Admin | see who changed what | I can trace mistakes |

> **Note:** US-8 is now satisfied for admins only (FR-16). Editors do not see lead data, which contains personal information. If editors need lead access, grant the `admin` role or export a CSV for them.

## 7. Functional Requirements
| ID | Requirement | Priority |
|---|---|---|
| FR-1 | Scroll position maps 1:1 to a video frame inside a sticky section | P0 |
| FR-2 | Canvas image-sequence renderer with `object-fit: cover` behavior | P0 |
| FR-3 | Preloader with % progress; scroll locked until ready | P0 |
| FR-4 | Text overlays fade in and out at configured progress ranges (0–1) | P0 |
| FR-5 | Smooth scrolling (Lenis) synced with ScrollTrigger | P1 |
| FR-6 | Separate mobile frame set (smaller resolution and count) | P0 |
| FR-7 | Reduced-motion fallback (static image + stacked text) | P0 |
| FR-8 | Lead capture form (email, optional name) with validation | P0 |
| FR-9 | Page content (scenes, overlays) loaded from the API, with a static JSON fallback | P1 |
| FR-10 | Analytics events: scroll milestones, CTA clicks, lead submits | P1 |
| FR-11 | Admin access to leads and content via the Supabase dashboard (fallback once FR-12 to FR-16 ship) | P2 |
| FR-12 | Invite-only editor sign-in via Supabase Auth magic link at `/admin` | P1 |
| FR-13 | Role-based authorization (`editor`, `admin`) enforced server-side on every `/api/admin/*` route | P1 |
| FR-14 | TOTP MFA required for the `admin` role | P1 |
| FR-15 | Audit log of all admin writes | P1 |
| FR-16 | Leads viewer, CSV export and delete (admin only) | P2 |

## 8. Non-Functional Requirements
| Category | Requirement |
|---|---|
| Performance | LCP < 2.5s, CLS < 0.05, frames total ≤ 12 MB desktop / ≤ 5 MB mobile |
| Smoothness | ≥ 55 fps while scrubbing on a 2022 mid-range Android |
| Compatibility | Last 2 versions of Chrome, Safari, Firefox, Edge; iOS 15+ |
| Accessibility | WCAG 2.2 AA |
| SEO | Real HTML text, meta and OG tags, Lighthouse SEO ≥ 95 |
| Security | Rate-limited lead endpoint, no secrets on the client, GDPR consent checkbox |
| Auth security | No public sign-up; sign-in responses never reveal whether an email is registered; admin routes are `no-store` and `noindex`; a disabled editor is blocked within 1h (token expiry) at the latest |
| Availability | 99.9% (static CDN hosting) |

## 9. Scope
**MVP (v1):** FR-1 to FR-8, FR-10
**v1.1:** FR-9, FR-11, FR-12 to FR-16 (see [Implementation Plan](./17-IMPLEMENTATION-PLAN.md) §8 for the option to pull auth into v1)
**v2:** Multiple scroll scenes per page, i18n, A/B testing of copy

## 10. Assumptions & Dependencies
- The client supplies a 3–8s source video at 1080p+ with no baked-in text.
- Hosting on Vercel; data and authentication on Supabase (Postgres + Auth).
- GSAP's standard license covers this use (free for this case).
- Editors and admins have email addresses that can receive magic links.
- The company owns the domain registrar account and can edit DNS (see [Domain, DNS & Email](./13-DOMAIN-DNS-EMAIL.md)).

## 11. Risks
| Risk | Impact | Mitigation |
|---|---|---|
| Large frame payload on slow networks | High bounce rate | Progressive loading, mobile set, AVIF |
| iOS Safari quirks | Broken scrub | Canvas method (not `<video>`) |
| Motion sickness | Accessibility complaints | `prefers-reduced-motion` fallback |
| Spam leads | Bad data | Rate limit + honeypot field |
| Editor account compromised | Content defaced, leads exposed | Invite-only, MFA for admins, audit log, short sessions |

## 12. Release Criteria
- All P0 requirements done and tested on the device matrix
- Lighthouse: Performance ≥ 85, Accessibility ≥ 95, SEO ≥ 95
- No P0/P1 bugs open
- Stakeholder sign-off on the visual QA
- For the release that includes auth: every `/api/admin/*` route has passing 401/403 tests, and the anon key cannot read `leads`, `editors` or `audit_log`

---
