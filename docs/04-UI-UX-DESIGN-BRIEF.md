<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 04 — Ui Ux Design Brief

# UI/UX Design Brief
**Related:** [PRD](./01-PRD.md) · [App Flow](./03-APP-FLOW.md)

## 1. Project Summary
Design a premium, cinematic landing page where the product video is the hero. The UI stays minimal so the motion and product stay in focus. A small functional admin area ships alongside it.

## 2. Objectives
1. **Wow in 3 seconds:** the first scroll should feel magical.
2. **Clarity:** each scroll stage communicates one idea.
3. **Conversion:** the story ends naturally at the CTA.

## 3. Audience
Tech-savvy consumers aged 22–45. About 60% mobile and 40% desktop. They expect Apple/Nothing-level polish. The admin area is used by a handful of internal staff on desktop.

## 4. Brand & Tone
| Attribute | Is | Is not |
|---|---|---|
| Voice | Confident, concise | Salesy, jargon-heavy |
| Visual | Dark, cinematic, spacious | Busy, gradient-heavy |
| Motion | Smooth, purposeful | Bouncy, gimmicky |

## 5. Design Principles
1. **Content is the video.** UI elements never compete with the frames.
2. **One message per screen.** Max 6 words per headline.
3. **Motion follows scroll.** No autoplay animation inside the scroll section.
4. **Readable everywhere.** Text always has contrast support (scrim or shadow).
5. **Admin is plain.** Functional, fast, accessible. It reuses the tokens and has no motion.

## 6. Design Tokens

### Color
| Token | Value | Use |
|---|---|---|
| `--bg` | `#000000` | Page background |
| `--surface` | `#111113` | Cards, form |
| `--fg` | `#F5F5F7` | Primary text |
| `--fg-muted` | `#86868B` | Secondary text |
| `--accent` | `#2997FF` | CTA, links, focus ring |
| `--danger` | `#FF453A` | Form errors |
| `--success` | `#30D158` | Success state |

### Typography (Inter / system)
| Style | Size (clamp) | Weight | Line height |
|---|---|---|---|
| Display | `clamp(2.5rem, 8vw, 6rem)` | 700 | 1.05 |
| H2 overlay | `clamp(2rem, 6vw, 5rem)` | 700 | 1.1 |
| H3 | `clamp(1.5rem, 3vw, 2.25rem)` | 600 | 1.2 |
| Body | `1.125rem` | 400 | 1.6 |
| Caption | `0.875rem` | 500 | 1.4 |

### Spacing & Radius
- Spacing scale: `4, 8, 12, 16, 24, 32, 48, 64, 96, 128 px`
- Radius: `8px` inputs, `16px` cards, `999px` buttons

## 7. Layout & Grid
| Breakpoint | Width | Columns | Gutter | Margin |
|---|---|---|---|---|
| Mobile | < 768px | 4 | 16px | 20px |
| Tablet | 768–1199px | 8 | 24px | 40px |
| Desktop | ≥ 1200px | 12 | 24px | auto (max 1200px) |

## 8. Motion Spec
| Element | Trigger | Animation | Easing |
|---|---|---|---|
| Canvas frames | Scroll (scrub 0.5s) | frame index | linear |
| Overlay in | Progress = `start` | opacity 0→1, y 40→0 | `power2.out` |
| Overlay out | Progress = `end − fade` | opacity 1→0, y 0→−40 | `power2.in` |
| Canvas end | 85–100% | scale 1→0.85, radius 0→24px | linear |
| Loader exit | Ready | opacity 1→0, 600ms | `ease` |
| Buttons | Hover | scale 1.03, 200ms | `ease-out` |
| Scroll hint | Idle | translateY 0→10px loop, 2s | `ease-in-out` |

**Reduced motion:** remove all transforms, fades ≤ 150ms, static poster.

## 9. Components
| Component | States | Notes |
|---|---|---|
| Header | default, scrolled (blur bg) | Fixed, 64px tall, hides on scroll down (mobile) |
| Loader | loading, stalled, exit | Thin 200px bar + % text |
| Overlay block | hidden, entering, visible, exiting | Center / left / right alignments |
| CTA button | default, hover, focus, active, disabled | Min 44×44px touch target |
| Input | default, focus, error, disabled | Label above, error below |
| Consent checkbox | unchecked, checked, error | Links to /privacy |
| Toast / inline status | success, error | `aria-live="polite"` |
| Sign-in form (admin) | default, sending, sent, error | Email field + button. The "sent" message is identical for every email. |
| MFA code input (admin) | default, error, verifying | 6 digits, `inputmode="numeric"`, `autocomplete="one-time-code"` |
| Data table (admin) | loading, empty, populated | Leads and audit log. Sticky header, keyboard navigable. |
| Role badge (admin) | editor, admin | Text label, never color alone |
| Confirm dialog (admin) | open, busy | Required for delete lead, disable editor |

## 10. Key Screens to Design
1. Loader (loading + stalled)
2. Intro hero
3. Scroll section at 0%, 25%, 50%, 75%, 100% (desktop + mobile)
4. Features section
5. Signup form (default, error, success)
6. Reduced-motion version (full page)
7. Footer + Privacy page
8. Admin sign-in (default, link sent, no access)
9. Admin MFA step
10. Admin dashboard, leads table, audit log (desktop only)

## 11. Accessibility Requirements
- Text contrast ≥ 4.5:1. Over video, use a `linear-gradient(transparent, rgba(0,0,0,.6))` scrim.
- Visible focus ring: `2px solid var(--accent)`, offset 3px.
- Respect `prefers-reduced-motion`.
- Overlay text is real HTML in reading order.
- Form fields have labels and errors linked via `aria-describedby`.
- Admin screens meet the same WCAG 2.2 AA bar: keyboard-only operation, labelled tables, focus returned after dialogs close.

## 12. Video Art Direction
- Dark or neutral background, so it blends with `--bg` at the edges.
- Subject centered with **safe zones**: keep the key product within the central 60% (for mobile `cover` cropping).
- Leave negative space where overlays appear (e.g. left third for left-aligned text).
- Slow, continuous camera motion. No hard cuts.

## 13. Deliverables
- [ ] Figma file: tokens, components, all screens (desktop 1440 + mobile 390)
- [ ] Prototype showing the scroll storyboard
- [ ] Exported poster image (final frame)
- [ ] Motion reference video (screen recording of prototype)
- [ ] Admin screens (desktop 1440 only)

---
