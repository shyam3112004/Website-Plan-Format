<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 24 — FILE & MEDIA MANAGEMENT

**Suggested filename:** `24-FILE-MEDIA-MANAGEMENT.md`  
**Purpose:** File validation, upload rules, media transformations, storage, delivery, and retention.

## 1. Scope
Describe source footage, generated frame sequences, posters, social images, icons, and any admin-uploaded media. The current asset-generation workflow is defined in `07-ASSET-PIPELINE.md`.

## 2. File inventory
| Asset | Accepted format | Location | Public/private | Retention |
|---|---|---|---|---|
| Master source video | MP4 / ProRes | `source/` | Private, not committed | Per rights policy |
| Desktop frames | WebP / AVIF | `public/frames/desktop/` | Public | Versioned |
| Mobile frames | WebP / AVIF | `public/frames/mobile/` | Public | Versioned |
| Poster | WebP | `public/posters/` | Public | Current + prior version |
| OG image | JPG | `public/posters/` | Public | Current |
| Rights documents | PDF / source files | `source/licenses/` | Private | Legal retention |

## 3. Upload validation
If uploads are introduced, specify MIME and extension allow-lists, maximum file size, dimensions, malware scanning, filename normalization, authorization, and rejection messages. Do not imply the current v1 editor uploads raw video unless scope is changed.

## 4. Processing pipeline
Document source inspection, frame extraction, responsive crops, WebP/AVIF encoding, poster generation, manifest generation, QA, and publication. Link to exact commands in the Asset Pipeline.

## 5. Storage and delivery
Specify Supabase Storage bucket purpose, public/private access, Vercel CDN behavior, immutable versioned paths, cache headers, and access controls.

## 6. Versioning and rollback
Never overwrite the live frame set in place. Define version naming, manifest checks, cutover procedure, rollback, and cleanup of unused versions.

## 7. Rights, retention, and deletion
Record source/license owner, permitted channels, attribution, expiry, takedown contact, and deletion proof.


---
