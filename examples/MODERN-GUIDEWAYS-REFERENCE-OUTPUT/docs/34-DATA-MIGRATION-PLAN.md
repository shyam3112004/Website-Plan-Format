# 34 — Data Migration Plan
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Scope
Migration may be minimal if content starts as version-controlled JSON. If Supabase is approved, migrate only published page content, product categories, frame-set metadata and enquiry records that the company explicitly wants in the site system.

## 2. Migration inventory
| Data | Source | Destination | Decision |
|---|---|---|---|
| Company description | Approved company profile | `content/home.json` or `pages` | Required |
| Product category names | Company profile | `products` or JSON | Required |
| Product specs | Approved technical sheets only | Product fields | Not yet supplied |
| Scroll overlays | Approved storyboard | JSON or `overlays` | After frame review |
| Frame count/paths | Asset audit | `manifest.json` / `frame_sets` | Pending audit |
| Enquiry records | New website submissions | `enquiries` | No legacy migration assumed |

## 3. Migration rules
- Do not import sample or synthetic data into production.
- Normalize product slugs but preserve the approved display names.
- Keep media versioned and referenced by stable paths.
- Record migration ID, source, row count and verification result.
- Make schema changes backward-compatible where possible.

## 4. Validation
Compare source and target row counts, required fields, links, publication status and asset paths. Test public content API and enquiry workflow in staging before production.

## 5. Rollback
Keep source JSON/profile content and the previous schema migration. If migration fails, revert the page config to the static fallback and restore database from backup or apply an approved forward fix.
