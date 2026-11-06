# 30 — Git & Version Control Workflow
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Repository rules
Use a company-controlled repository. Commit source code, documentation, manifests and scripts. Do not commit `.env.local`, service-role keys, private enquiry exports, unlicensed source media or large generated frame sets unless repository policy explicitly allows LFS.

## 2. Branch model
- `main`: production-ready.
- `feat/<topic>`: feature work.
- `fix/<topic>`: bug fixes.
- `docs/<topic>`: documentation changes.
- `assets/<version>`: frame/product asset updates when needed.

## 3. Commit convention
Use `feat:`, `fix:`, `docs:`, `perf:`, `test:`, `chore:` prefixes. Keep a commit focused and include issue/context.

## 4. Pull request checklist
- [ ] Scope and screenshot/video preview included where relevant.
- [ ] Tests pass.
- [ ] Accessibility and reduced-motion impact reviewed.
- [ ] Frame manifest updated if media changed.
- [ ] No secrets or unapproved business claims.
- [ ] Docs and changelog updated.
- [ ] Rollback path understood.

## 5. Release tags
Tag production releases and record frame-set version and database migration version. Never rewrite published frame assets under an existing immutable URL.
