<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 35 — USER GUIDE, FAQ & TROUBLESHOOTING

**Suggested filename:** `35-USER-GUIDE-FAQ-TROUBLESHOOTING.md`  
**Purpose:** End-user instructions for visitors, editors, admins, and support staff.

## 1. Audience and scope
Separate public visitor help from internal editor/admin instructions. Do not expose admin-only operational details in public help.

## 2. Visitor guide
Explain how to scroll through the story, use the CTA, submit the lead form, access privacy/terms pages, and use the reduced-motion/static experience.

## 3. Editor guide
Link to `10-CONTENT-EDITOR-GUIDE.md`. Explain sign-in, editing overlay text and timing, previewing, publishing, and requesting a video/frame update.

## 4. Admin guide
Link to `06-AUTHENTICATION.md`. Explain invitations, MFA, role management, leads access, CSV export, deletion requests, and audit log access.

## 5. FAQ
| Question | Answer | Audience |
|---|---|---|
| Why is the video not moving? | Check reduced-motion settings; the site may intentionally show the static experience. | Visitor |
| Why is the page loading slowly? | Explain frame loading and the continue fallback; provide support contact. | Visitor |
| Why can I not see leads? | Leads are restricted to admins. | Editor |
| Why did my content change not update search previews? | SEO metadata is injected at build time; a redeploy is required. | Editor |

## 6. Troubleshooting format
For each issue record symptoms, likely cause, safe checks, resolution, escalation contact, and related runbook. Never ask users to share passwords, tokens, or sensitive lead data.

## 7. Support and feedback
Provide approved support channels, expected response times, escalation policy, and document owner/review date.


---

---
