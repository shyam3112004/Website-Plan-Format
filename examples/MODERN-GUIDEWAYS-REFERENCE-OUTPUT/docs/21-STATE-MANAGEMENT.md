# 21 — State Management
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. State inventory
| State | Owner | Persistence |
|---|---|---|
| Current frame index | Scroll controller | None; derived from scroll progress |
| Frame loading progress | Frame loader | Session only |
| Selected frame set | Initialization logic | Fixed for page session |
| Reduced-motion preference | Browser media query | OS/browser setting |
| Mobile navigation open | Header component | None |
| Enquiry form fields | Contact form | In memory only; do not persist without explicit design |
| Enquiry submission status | Contact form | In memory only |
| Optional consent choice | Consent manager if needed | Only as legally approved |

## 2. Ownership rules
Do not duplicate scroll progress in multiple components. The scroll controller owns normalized progress; renderer derives frame index; overlays derive visibility from the same progress. Form status is local to the form.

## 3. State machines
Frame loader: `idle → loading → keyframes-ready → complete` with `timeout`, `partial` and `error` branches. Enquiry: `idle → validating → submitting → success/error`. Mobile menu: `closed ↔ open`.

## 4. Race conditions
Ignore stale frame decode completions after the user scrolls away. Debounce resize. Prevent duplicate form submissions while in `submitting`. Use abort signals for obsolete requests.

## 5. Persistence policy
Do not persist contact form text in local storage. Do not persist frame index across sessions unless user research identifies a need. Keep preference handling limited to browser reduced-motion settings and any approved consent storage.

## 6. Tests
Test rapid forward/back scrolling, resize during loading, slow network, missing frame, API timeout, repeated submit, double-click CTA and reduced-motion change.
