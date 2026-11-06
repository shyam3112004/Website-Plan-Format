# 05 — Backend Schema
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Backend scope
The public marketing site can be static. A backend is justified for enquiry storage/notification, analytics and later editor workflows. Do not build a large CMS before the company confirms it needs one. The preferred backend from the supplied format is Supabase Postgres with server-side Vercel functions.

## 2. Proposed entities
| Table | Purpose | MVP status |
|---|---|---|
| `pages` | Page slug, title, metadata, publish state | Optional if content is static JSON |
| `products` | Approved product category text and order | Optional; can start as JSON |
| `scenes` | Scroll-story sequence and frame-set association | Needed only if content is database-driven |
| `frame_sets` | Base paths, counts, format, version and checksum | Can begin as a checked manifest |
| `overlays` | HTML text timing and CTA configuration | Needed only for editable overlays |
| `enquiries` | Quote requests submitted through the site | Yes if form data is stored |
| `analytics_events` | Minimal event tracking | Optional and consent-dependent |
| `editors` | Invite-only content editor mapping | Later release |
| `audit_log` | Records admin/editor changes | Later release |

## 3. Enquiry fields
```sql
create table public.enquiries (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  company_name text,
  email text,
  phone text,
  product_interest text not null,
  machine_type text,
  message text not null,
  consent_at timestamptz not null,
  created_at timestamptz not null default now(),
  status text not null default 'new'
    check (status in ('new','contacted','quoted','closed','spam')),
  source_path text,
  ip_hash text
);
```

Before applying, confirm that at least one of email/phone is required and implement a database check or server validation accordingly. Keep the schema migration in version control. Avoid storing raw IP addresses unless a documented legal/security need exists.

## 4. Content models
Product categories must initially use only these profile-backed names: Telescopic Covers, Roller Covers, Apron Covers, Bellows, Guide Wipers, Enclosing Guards. Add other CNC parts only after a product owner approves exact category names and descriptions. Technical specs belong in structured fields only after verified data sheets are supplied.

## 5. Security and access
- Public visitors can only submit an enquiry through a server endpoint.
- Anonymous clients cannot SELECT enquiries, editor records or audit logs.
- The server validates inputs, rate-limits submissions and uses the private DB key.
- Use Row Level Security with deny-by-default policies.
- Only admins may export or delete enquiry data.
- Editors may update published site copy only if the owner approves the custom admin portal.

## 6. Retention and deletion
Retention duration, deletion workflow and backup retention are pending company/legal approval. The site owner must be able to locate and delete an enquiry after a verified request. Backups must not be treated as a way to keep deleted records indefinitely; document how backup expiry works.

## 7. Seed content
Seed only company-approved page text and the six product-category records. Do not insert sample customers, certifications, prices or specifications that could accidentally appear on production.

## 8. Migration rules
Use numbered SQL migrations, apply to staging first, verify data and rollback plan, then production. See `34-DATA-MIGRATION-PLAN.md`.
