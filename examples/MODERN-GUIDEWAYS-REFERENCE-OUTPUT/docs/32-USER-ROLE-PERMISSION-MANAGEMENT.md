# 32 — User, Role & Permission Management
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Scope
No visitor registration. Public visitors can read public pages and submit a validated enquiry. Internal roles are introduced only if the company approves an admin/editor dashboard.

## 2. Permission matrix
| Action | Visitor | Editor | Admin |
|---|---:|---:|---:|
| View public site | Yes | Yes | Yes |
| Submit enquiry | Yes | Yes | Yes |
| Edit draft copy | No | Yes | Yes |
| Publish copy | No | By explicit policy | Yes |
| Read enquiries | No | No by default | Yes |
| Delete/export enquiries | No | No | Yes |
| Invite/disable users | No | No | Yes |
| View audit log | No | No | Yes |

## 3. Lifecycle
Create access only for named work identities. Require MFA where available, review access quarterly, and revoke immediately when no longer needed. Never share passwords or use a common admin account.

## 4. Enforcement
Enforce permissions on the server/database, not just by hiding buttons in the UI. Test unauthenticated and insufficient-role access for every admin route.

## 5. Audit
Record actor, action, resource ID, timestamp and request ID for privileged changes. Keep enquiry contents out of audit metadata.
