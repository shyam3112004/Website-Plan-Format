<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 14 — Seo And Discoverability

# SEO & Discoverability
**Related:** [PRD](./01-PRD.md) §8 · [TRD](./02-TRD.md) §3.1 · [Domain, DNS & Email](./13-DOMAIN-DNS-EMAIL.md) · [QA Checklist](./09-QA-CHECKLIST.md) §8 · [Analytics Spec](./12-ANALYTICS-SPEC.md)

## 1. Scope and Goals
| Goal | Target |
|---|---|
| Lighthouse SEO | ≥ 95 |
| Page text readable without running the video or the API | Yes |
| Correct title, description and preview image when the link is shared | Yes |
| Indexed by Google and Bing within 2 weeks of launch | Yes |

The site is one main page, so SEO here is about correctness and discoverability, not a content strategy.

## 2. Page Inventory
| URL | Indexed | Notes |
|---|---|---|
| `/` | Yes | Main page |
| `/privacy`, `/terms` | Yes | Static HTML files in `public/` |
| `/admin`, `/admin/*` | **No** | `noindex` header ([Runbook §4](./08-DEPLOYMENT-RUNBOOK.md)). Do not list in the sitemap. Do **not** block it in `robots.txt`, or crawlers cannot see the `noindex` header. |
| `/api/*` | No | Disallowed in `robots.txt` |
| `/404` | No | Served for unknown URLs (§8) |

## 3. Build-Time Injection
Search and social crawlers read the **HTML as served**. Many do not run JavaScript, and the title, description and overlay text live in the database. So they are written into `index.html` **at build time**.

1. `npm run export:content` writes `public/content/home.json`. In addition to the `PageConfig`, the file carries two static-only fields read from `pages`: `metaDescription` and `ogImageUrl`.
2. A small Vite plugin reads that file during the build and fills markers in `index.html`.

```html
<!-- index.html -->
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <!--SEO_HEAD-->
</head>
<body>
  ...
  <noscript><main><!--NOSCRIPT_TEXT--></main></noscript>
</body>
```
```js
// scripts/inject-content.mjs  (add to the plugins array in vite.config.js)
import { readFileSync } from 'node:fs';

const esc = (s = '') => String(s).replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/"/g, '&quot;');

export function injectContent(file = 'public/content/home.json') {
  return {
    name: 'inject-content',
    transformIndexHtml(html) {
      const c = JSON.parse(readFileSync(file, 'utf8'));
      const site = process.env.SITE_URL;               // https://example.com, no trailing slash
      const head = [
        `<title>${esc(c.title)}</title>`,
        `<meta name="description" content="${esc(c.metaDescription)}" />`,
        `<link rel="canonical" href="${site}/" />`,
        `<meta property="og:type" content="website" />`,
        `<meta property="og:title" content="${esc(c.title)}" />`,
        `<meta property="og:description" content="${esc(c.metaDescription)}" />`,
        `<meta property="og:url" content="${site}/" />`,
        `<meta property="og:image" content="${site}${esc(c.ogImageUrl)}" />`,
        `<meta name="twitter:card" content="summary_large_image" />`,
      ].join('\n  ');
      const text = c.scenes.flatMap((s) => s.overlays)
        .map((o) => `<h2>${esc(o.heading)}</h2>${o.body ? `<p>${esc(o.body)}</p>` : ''}`)
        .join('\n');
      return html.replace('<!--SEO_HEAD-->', head).replace('<!--NOSCRIPT_TEXT-->', text);
    },
  };
}
```
Consequences:
- Changing the page title, description or OG image in the database does **not** change the HTML until the next build. Trigger a redeploy after editing them.
- Overlay text is in the HTML for no-JavaScript visitors and simple crawlers. Googlebot runs JavaScript and will also see the text the page renders.
- Run `npm run export:content` before every production build (add it to the build command or the CI step).

## 4. Head Tags
| Tag | Rule |
|---|---|
| `<title>` | Up to 60 characters. `[Primary message] \| [Company]` |
| Meta description | Up to 155 characters, one clear benefit and a call to action |
| Canonical | Absolute URL of the canonical host |
| `og:image` | **Absolute** URL, 1200×630 (`/posters/og.jpg`, [Asset Pipeline](./07-ASSET-PIPELINE.md)) |
| `twitter:card` | `summary_large_image` |
| Headings | One `<h1>` (the hero title), then `<h2>` per overlay |
| Images | Meaningful `alt` text; the poster described, decorative images `alt=""` |
| `lang` | `<html lang="en">` |

Also keep these static tags in `index.html`:
```html
<meta name="theme-color" content="#000000" />
<link rel="icon" href="/favicon.ico" sizes="32x32" />
<link rel="icon" href="/icon.svg" type="image/svg+xml" />
<link rel="apple-touch-icon" href="/apple-touch-icon.png" />
<link rel="manifest" href="/site.webmanifest" />
```

## 5. `robots.txt` and Sitemap
```
# public/robots.txt
User-agent: *
Allow: /
Disallow: /api/
Sitemap: https://example.com/sitemap.xml
```
```xml
<!-- public/sitemap.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url><loc>https://example.com/</loc><lastmod>2026-11-27</lastmod></url>
  <url><loc>https://example.com/privacy</loc></url>
  <url><loc>https://example.com/terms</loc></url>
</urlset>
```
Replace the domain and dates. Set `"cleanUrls": true` in `vercel.json` ([Runbook §4](./08-DEPLOYMENT-RUNBOOK.md)) so `privacy.html` is served at `/privacy`. Update `lastmod` when the content changes.

## 6. Structured Data
Add one JSON-LD block to `index.html`. It is a data block, so the public CSP does not affect it.
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "[Company]",
  "url": "https://example.com",
  "logo": "https://example.com/icon-512.png"
}
</script>
```
Validate it with a structured-data testing tool after every change.

## 7. Favicons and Icons
| File | Size | Used for |
|---|---|---|
| `favicon.ico` | 32×32 | Browsers |
| `icon.svg` | Vector | Modern browsers |
| `apple-touch-icon.png` | 180×180 | iOS home screen |
| `icon-192.png`, `icon-512.png` | 192, 512 | Web manifest and structured data |
| `site.webmanifest` | | Name, icons, `theme_color` |

## 8. Error Pages and Redirects
- `public/404.html` in the site's style: a short message and a link home. Vercel serves it for unknown URLs with a 404 status.
- One canonical host: the other host redirects with 308 ([Domain §4](./13-DOMAIN-DNS-EMAIL.md)).
- If the domain replaces an older site, map every old URL to its new target ([Launch & Go-Live §4](./15-LAUNCH-AND-GO-LIVE.md)). Use permanent redirects, never "everything to the homepage".

## 9. UTM Convention
Campaign links carry UTM parameters. The lead form stores `utm_source` in `leads.source` and the full set in `leads.utm`, and `page_view` records `utmSource` ([Analytics Spec §2](./12-ANALYTICS-SPEC.md)).

| Parameter | Rule | Examples |
|---|---|---|
| `utm_source` | Where the link lives | `instagram`, `newsletter`, `partner-name` |
| `utm_medium` | Type of channel | `social`, `email`, `cpc`, `referral` |
| `utm_campaign` | Campaign and month | `launch-2026-11` |

Lowercase only, words separated by hyphens, never spaces. Keep a shared sheet of live links.

## 10. Search Console and Bing Webmaster
1. After launch, add the domain as a **Domain property** (verified with the DNS TXT record, [Domain §6](./13-DOMAIN-DNS-EMAIL.md)).
2. Submit `https://<domain>/sitemap.xml`.
3. Request indexing for `/`.
4. Check the Page indexing report after 3 days and again after 2 weeks.
5. Once there is enough traffic, watch the Core Web Vitals report. It uses real-user data, which complements Lighthouse CI.
6. Repeat for Bing Webmaster Tools.

## 11. SEO Checks
These are run as part of the [QA Checklist §8](./09-QA-CHECKLIST.md) (S-01 to S-09).

| Check | Pass |
|---|---|
| View source of the built page | Title, description, canonical, OG tags and overlay text are present without running JavaScript |
| Share the URL in a chat app and on a social network | Correct title, description and image |
| `robots.txt` and `sitemap.xml` open at the root | Yes; sitemap lists only public pages |
| Structured data validates | No errors |
| `/admin` | `noindex`, absent from the sitemap |
| Unknown URL | Custom 404 with a 404 status |

---
