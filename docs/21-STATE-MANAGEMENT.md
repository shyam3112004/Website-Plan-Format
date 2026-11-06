<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 21 — STATE MANAGEMENT

**Suggested filename:** `21-STATE-MANAGEMENT.md`  
**Purpose:** Client-side state, server state, cache rules, persistence, and state transitions.

## 1. State inventory
| State | Owner | Lifetime | Persistence | Source of truth |
|---|---|---|---|---|
| Page configuration | Content loader | Page session | Static JSON fallback | API / fallback JSON |
| Frame preload status | Canvas module | Page session | None | Image load events |
| Scroll progress | Scroll controller | Current page | None | Scroll position |
| Overlay visibility | Overlay module | Current page | None | Scroll progress + config |
| Lead form status | Lead form module | Current page | None | API response |
| Admin session | Supabase Auth | Session | Supabase-managed | Supabase Auth |

## 2. State ownership rules
Each state has one owner. Avoid duplicated state between modules. Document which values are derived and must not be stored independently.

## 3. State machines
Add diagrams for loader, lead form, content editor save/publish, and authentication states. Define valid transitions and rejected transitions.

## 4. Persistence
Document session-only values such as the analytics session ID, authentication-managed tokens, and values that must never be stored in local storage. Do not persist sensitive lead data in the browser.

## 5. Server cache and invalidation
Specify cache headers for public content, admin responses, static assets, and fallback JSON. Document when publishing content takes effect.

## 6. Loading, empty, and error states
For every asynchronous operation define loading UI, timeout, retry behavior, empty state, recoverable error, and fatal error.

## 7. Race conditions and concurrency
Document stale responses, duplicate submits, rapid scroll/resize events, cancellation, and editor simultaneous updates.

## 8. Test cases
Provide transition tests, persistence tests, stale-response tests, and reduced-motion tests.


---
