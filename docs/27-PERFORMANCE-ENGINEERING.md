<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 27 — PERFORMANCE ENGINEERING

**Suggested filename:** `27-PERFORMANCE-ENGINEERING.md`  
**Purpose:** Performance budgets, measurement, optimization, and regression controls.

## 1. Goals and budgets
Use the targets in the PRD and TRD as the baseline. Record measured values separately from targets.

## 2. Performance budget
| Metric | Target | Measurement | Release gate |
|---|---|---|---|
| LCP | PRD target: < 2.5 s | Field + lab | Required |
| Scroll rendering | Target: 60 fps on mid-range devices | Frame timing / profiling | Required |
| Desktop frame set | Asset Pipeline cap: 12 MB per format | Manifest audit | Required |
| Mobile frame set | Asset Pipeline cap: 5 MB per format | Manifest audit | Required |
| JS bundle | `[set budget]` | Build analyzer | Pending decision |
| API latency | `[set target]` | Production telemetry | Pending decision |

## 3. Measurement protocol
Record device model, browser, network profile, cache state, viewport, test URL, tool/version, date, and repeated-run statistics.

## 4. Optimization areas
Cover critical CSS, JS splitting, frame preload priority, responsive frame selection, AVIF/WebP negotiation, canvas draw scheduling, memory management, CDN caching, API payload size, and database indexes.

## 5. Regression prevention
Define CI checks, Lighthouse runs, asset-manifest checks, bundle-size thresholds, and performance review for every release.

## 6. Results log
| Date | Build | Device/network | Metric | Result | Pass/fail | Issue |
|---|---|---|---|---|---|---|
| `[date]` | `[commit]` | `[profile]` | `[metric]` | `[value]` | `[status]` | `[ticket]` |


---
