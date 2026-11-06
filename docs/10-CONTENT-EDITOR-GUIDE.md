<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 10 — Content Editor Guide

# Content Editor Guide
**Audience:** Marketing editors (no coding needed) · **Related:** [PRD](./01-PRD.md) (US-7, US-9) · [Backend Schema](./05-BACKEND-SCHEMA.md) · [Authentication](./06-AUTHENTICATION.md)

## 1. What You Can Change
| You can | You cannot (ask a developer) |
|---|---|
| Overlay headline, body text, CTA label and link | Video frames, frame counts, `base_path` |
| When each overlay appears and disappears | Scroll length of a scene |
| Overlay alignment (center, left, right) | Page slug, layout, colors, fonts |
| Page title and meta description (search and social tags update at the next site build, so ask a developer to redeploy) | Anything in `leads`, `editors` or `audit_log` |
| Publish or unpublish a page | Replace the video (needs the [Asset Pipeline](./07-ASSET-PIPELINE.md)) |

Leads are visible to **admins** only, because they contain personal data.

## 2. The Rules
The database rejects a save that breaks these rules.

| Field | Rule |
|---|---|
| Headline (`heading`) | Required, up to 80 characters. Aim for 6 words or fewer. |
| Body (`body`) | Optional, up to 240 characters |
| `start_progress`, `end_progress` | Numbers from 0 to 1. `end_progress` must be larger than `start_progress`. |
| CTA | Fill **both** `cta_label` and `cta_href`, or leave both empty |
| `align` | `center`, `left` or `right` |

### How timing works
The numbers mean how far through the scroll section the visitor is. `0` is the start and `1` is the end.

| Value | Position |
|---|---|
| 0.25 | A quarter of the way |
| 0.50 | Halfway |
| 0.75 | Three quarters |
| 1.00 | The end |

The default content, as an example:

| Headline | Start | End |
|---|---|---|
| Introducing X | 0.05 | 0.25 |
| Designed for performance | 0.30 | 0.50 |
| Every detail matters | 0.55 | 0.75 |
| Available now (+ CTA) | 0.80 | 1.00 |

Leave small gaps between overlays so only one message is on screen at a time. A short overlay (under 0.10) goes by too fast to read.

## 3. Writing Guidance
- One idea per overlay. Short, confident sentences. No jargon.
- Write for a phone first. If the headline needs more than two lines on a phone, shorten it.
- A CTA label is a verb: "Buy now", "Join the list". The link is usually `#signup`.
- Don't put the key message in the first or last 3% of the scroll, where it is easy to miss.

## 4. Editing Today (Supabase dashboard, before `/admin` ships)
1. Open the Supabase project and go to **Table Editor**.
2. **Change text or timing:** open `overlays`, click the cell, edit, save.
3. **Add an overlay:** in `overlays`, insert a row. Copy the `scene_id` from an existing overlay. Fill the fields from §2.
4. **Remove an overlay:** delete its row.
5. **Page title or description:** edit the row in `pages`. The search and social tags are fixed at build time, so ask a developer to redeploy for them to change.
6. **Take a page offline:** set `pages.status` to `draft`. Set it back to `published` to restore it.

## 5. Editing After v1.1 (`/admin`)
1. Go to `/admin`, enter your work email and open the link we send. The message is the same whether or not your email is registered.
2. Open the page, change the overlays, and check the preview.
3. Click **Publish**.
4. Sign out when you're done. Admins are asked for a one-time code from their authenticator app.

If you can't sign in, ask an admin to check that you're invited and not disabled.

## 6. Checking Your Change
- A published change shows up on the live site within about 5 minutes. When traffic is very low it can take longer. Reload the page twice.
- Check it on a phone and on a laptop.
- If the site's data service is down, visitors see a saved copy of the content. A developer refreshes that copy (`npm run export:content`) after big copy changes. Ask for it after a major rewrite.

## 7. Troubleshooting
| Problem | Likely cause | What to do |
|---|---|---|
| Save is rejected with a "check constraint" message | A rule in §2 is broken | Check lengths, that end is larger than start, and that the CTA fields are both filled or both empty |
| Change isn't visible | Cache, or the page isn't `published` | Wait 5 minutes and reload twice. Check `pages.status`. |
| An overlay never appears | Start and end are too close, or the overlay overlaps another | Make the range at least 0.10 wide and leave a gap |
| The CTA doesn't scroll to the form | The link is wrong | Use `#signup` |
| Can't see leads | Editors don't have access | Ask an admin for an export |

## 8. Known Limitation
Editing a **published** page changes the live site. The schema has no separate draft version of a live page. To prepare a big rewrite privately, ask a developer for a copy on a staging site. Revisions and staging for live pages are planned for v2 (see the [Implementation Plan](./17-IMPLEMENTATION-PLAN.md) §7).

---
