# 10 — Content Editor Guide
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Content ownership
Business owner approves all claims, product names, company details, technical statements and images. A designated editor may update approved copy after the owner signs off the content workflow.

## 2. Approved starting content
- Business: Modern Guideways.
- Primary focus: CNC, VMC and HMC telescopic-cover service and manufacturing.
- Categories: telescopic covers, roller covers, apron covers, bellows, guide wipers and enclosing guards.
- Company profile: custom designs, CNC parts design, workmanship and raw-material quality are described in the supplied profile.
- Experience: the supplied profile states 20 years' experience in telescopic-cover design; confirm the wording before publication.
- Vision/mission themes: precision, reliability, continuous innovation, quality, safety, on-time delivery, customer satisfaction and capability development.

## 3. Writing rules
- Use “Modern Guideways” consistently.
- Prefer “telescopic covers” unless the owner confirms a different standard term.
- Avoid unsupported claims, guarantees, certifications, tolerances, materials or capacity figures.
- Keep headings concise and product descriptions factual.
- Do not put essential text inside animation frames; overlay it as HTML.
- Each product image must show the product named in its caption.

## 4. Scroll overlay timing
Overlays use normalized progress from `0` to `1`. A start/end pair controls when an overlay is visible. Keep text within safe zones identified on the actual contact sheet. Avoid changing copy and timing in the same release unless reviewed together.

## 5. Product content template
```text
Product name:
Approved short description:
Application (owner-confirmed):
Available customization (owner-confirmed):
Photo filename:
Alt text:
CTA label:
Approver/date:
```

## 6. Editing workflow
1. Create a content change request.
2. Confirm source/evidence for claims and images.
3. Edit local JSON or approved CMS draft.
4. Preview desktop, mobile and reduced-motion states.
5. Check links and form CTA.
6. Obtain owner approval.
7. Publish through the deployment workflow.
8. Record the change in `CHANGELOG.md`.

## 7. Frame changes
A video/frame update is an asset release, not a copy edit. Follow `07-ASSET-PIPELINE.md`, create a new version folder, update the manifest, review the contact sheet and test rollback.

## 8. Troubleshooting
- Overlay appears too early/late: inspect normalized `start` and `end` values.
- Text hides a product: move overlay to a safe zone identified in contact sheet.
- Product crop is wrong on phone: use an approved mobile crop or static product image.
- Content does not update: check cache policy and redeploy/revalidate according to the chosen content source.
