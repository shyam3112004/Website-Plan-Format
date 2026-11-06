# 28 — Disaster Recovery & Backup Plan
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Critical assets and systems
- Source video and original extracted frame sequence.
- Approved logo, product photographs and rights documents.
- Git repository and deployment configuration.
- Domain registrar and DNS records.
- Supabase database and migrations, if used.
- Enquiry notification provider and approved recipient.

## 2. Recovery objectives
Set recovery time and recovery point objectives with the business owner based on how much enquiry data is acceptable to lose. These values are pending; do not assume provider backups alone satisfy the requirement.

## 3. Backup policy
Keep source media in at least two controlled locations. Keep Git as versioned source, export production database backups on an owner-approved cadence, and document retention/encryption/access. Do not store secrets in backup folders without a secure vault.

## 4. Restore procedure
1. Confirm incident scope and owner approval.
2. Restore repository/deployment from last known-good commit.
3. Restore DB to a staging copy first, if applicable.
4. Verify enquiry rows, migrations and permissions.
5. Restore frame manifest and matching versioned media.
6. Promote restored service and run smoke tests.

## 5. Recovery tests
Perform a restore test at least quarterly when a database is used. Verify source media can be opened and the frame sequence rebuilt from its archived originals.

## 6. Incident communication
Identify business owner, technical owner, affected data, current status, user impact and next update channel. Preserve relevant logs and document lessons learned.
