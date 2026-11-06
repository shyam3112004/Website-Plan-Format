<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 18 — FRONTEND ARCHITECTURE

**Suggested filename:** `18-FRONTEND-ARCHITECTURE.md`  
**Purpose:** Frontend application structure, module boundaries, rendering lifecycle, routing, and coding conventions.

## 1. Purpose and scope
Describe the browser application and its responsibilities. Keep public visitor code separate from the admin entry point.

## 2. Frontend architecture
Document the runtime flow from `index.html` to `main.js`, content loading, frame loading, canvas rendering, overlays, lead form, and analytics. Include a Mermaid component diagram.

## 3. Application entry points
| Entry | File | Audience | Responsibilities |
|---|---|---|---|
| Public | `index.html` | Visitors | Landing page and scroll experience |
| Admin | `admin.html` | Invited staff | Sign-in and content administration |

## 4. Source and folder structure
Record the actual `src/` tree. For every module specify its purpose, public exports, dependencies, and owner. Keep the documented tree synchronized with the repository.

## 5. Rendering lifecycle
Specify initialization order, config retrieval, frame-set selection, preload strategy, canvas sizing, draw scheduling, resize handling, and cleanup.

## 6. Routing and navigation
Document supported public routes, admin routes, anchors, 404 behavior, and direct-navigation behavior. State which routes are `noindex`.

## 7. Browser state and error boundaries
Define loading, ready, stalled, reduced-motion, offline, API-failure, and empty-content states. Explain the user-visible fallback for each.

## 8. Security boundaries
The browser must not contain the Supabase service-role key or query Postgres directly. Document which calls are public and which require server-side authorization.

## 9. Performance constraints
Record bundle budgets, frame loading budgets, rendering targets, lazy-loading rules, and measurement method. Link to Performance Engineering.

## 10. Testing strategy
List unit, integration, browser, visual-regression, and device tests. Link each critical requirement to a test ID.

## 11. Decisions and open questions
| ID | Decision / question | Owner | Status | Due date |
|---|---|---|---|---|
| FE-001 | `[decision]` | `[owner]` | Open | `[date]` |


---
