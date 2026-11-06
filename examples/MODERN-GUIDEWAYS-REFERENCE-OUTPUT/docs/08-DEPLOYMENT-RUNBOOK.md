# 08 — Deployment Runbook
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Environments
- Local: developer workstation, local `.env` only.
- Preview/staging: isolated deployment and non-production database/project.
- Production: `www.modernguideways.com` or the owner-approved canonical host after DNS verification.

Do not assume the domain is already controlled by the project team because it is printed on the profile.

## 2. One-time setup
1. Create the repository under a company-controlled GitHub organization/account.
2. Create Vercel project and connect the repository.
3. If Supabase is approved, create separate staging and production projects.
4. Add environment variables in the deployment provider; keep secrets out of Git.
5. Set the build command and output directory based on the finalized Vite configuration.
6. Configure preview protection if staging contains non-public business information.

## 3. Build and deploy
```bash
npm install
npm run dev
npm run lint
npm test
npm run build
npm run preview
```

Use `vercel dev` for local testing of Vercel serverless API routes. The exact script names must match the committed `package.json`.

## 4. Production headers
Configure security headers appropriate to the final deployment: HTTPS, content type sniffing protection, referrer policy, a carefully tested Content Security Policy, and admin `noindex`/`no-store` if an admin route exists. Do not paste a generic CSP without testing GSAP, fonts, image sources and Supabase endpoints.

## 5. Cache policy
- Versioned frames and static image assets may use long-lived immutable cache headers.
- HTML and content config should revalidate so updates appear.
- API page config may use a short cache only after publish behavior is tested.
- Admin and enquiry responses must not be publicly cached.

## 6. Release procedure
1. Review merged pull request and changelog.
2. Apply backward-compatible database migration to staging.
3. Run unit, contract, E2E, accessibility and frame-manifest tests.
4. Review preview URL on desktop and mobile.
5. Confirm enquiry destination and deliverability.
6. Apply production migrations, if any.
7. Deploy production build.
8. Run smoke tests and verify logs/analytics.
9. Keep the previous deployment available for rollback.

## 7. Rollback
Rollback the deployment to the previous known-good build. If frame URLs changed, switch the content manifest to the previous version. If a database migration is involved, use the documented backward-compatible rollback or forward-fix plan; never blindly drop production data.

## 8. Monitoring
Monitor HTTP error rate, frame asset 404s, enquiry API failures, form success count, page load performance and uptime. Assign a named on-call owner before launch.

## 9. Secrets and access
Use MFA on GitHub, Vercel, DNS registrar and Supabase accounts. Store secrets only in approved environment-variable managers. Rotate any credential exposed in a repository, chat or screenshot.

## 10. Deployment acceptance
The canonical domain resolves to production over HTTPS, all major routes return expected status codes, frame sequence loads, phone/email links work, enquiry test reaches the approved inbox/database, SEO tags are present and rollback has been tested on preview/staging.
