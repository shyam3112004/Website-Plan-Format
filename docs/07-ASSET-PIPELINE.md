<!-- Reusable source-format document. Treat this as a STRUCTURE AND COVERAGE REFERENCE. -->
<!-- Adapt all project-specific names, dates, providers, architecture, limits and examples to the current website brief. -->
<!-- Do not copy a sample technology decision unless it fits the current project. Mark unknown facts as TBD and explain applicability. -->

# 07 — Asset Pipeline

# Asset Pipeline
**Related:** [TRD](./02-TRD.md) §4–5 · [Backend Schema](./05-BACKEND-SCHEMA.md) (`frame_sets`) · [Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md)

## 1. Purpose
Turn one source video into the files the site needs: a desktop frame set, a mobile frame set (each in WebP and AVIF), a poster, an OG image and an AVIF probe file. Everything is produced by two scripts so any developer can repeat it.

| Output | Location | Consumed by |
|---|---|---|
| Desktop frames | `public/frames/desktop/frame_0001.webp` / `.avif` | `scrollCanvas.js`, `frame_sets.base_path` |
| Mobile frames | `public/frames/mobile/frame_0001.webp` / `.avif` | same |
| Poster | `public/posters/hero.webp` | Reduced-motion mode, `<noscript>`, `scenes.poster_url` |
| OG image | `public/posters/og.jpg` (1200×630) | Social previews |
| AVIF probe | `public/probe.avif` | Format detection (§6) |
| Manifest | `public/frames/manifest.json` | Updating `frame_sets` (§8) |

## 2. Source Video Requirements
| Item | Requirement |
|---|---|
| Length | 3–8 seconds |
| Resolution | 1080p or higher, 16:9 |
| Frame rate | Constant (no variable frame rate) |
| Content | No baked-in text. Dark or neutral background so edges blend with `--bg`. |
| Framing | Key subject inside the central 60% (mobile crops with `cover`). Negative space where overlays appear. |
| Motion | Slow, continuous camera movement. No hard cuts. |
| Format | H.264 MP4 or ProRes. Keep the original in `source/` (not committed). |

## 3. Budgets
| Set | Frames | Width | Target per frame | Total cap |
|---|---|---|---|---|
| Desktop | 150 | 1920 px | ~80 KB | 12 MB |
| Mobile | 120 | 960 px | ~40 KB | 5 MB |

The caps apply to **each format** (WebP and AVIF). Only one format is downloaded per visitor.

## 4. Export Script
```bash
#!/usr/bin/env bash
# scripts/export-frames.sh
# Usage: scripts/export-frames.sh <source.mp4> [version]
# Without [version] the output goes to public/frames/{desktop,mobile}.
# With a version (e.g. v2) it goes to public/frames/v2/{desktop,mobile}.
set -euo pipefail

SRC="${1:?source video required}"
VER="${2:-}"
OUT="public/frames${VER:+/$VER}"
TMP="$(mktemp -d)"
trap 'rm -rf "$TMP"' EXIT

DESKTOP_COUNT=150; DESKTOP_W=1920; DESKTOP_Q=75
MOBILE_COUNT=120;  MOBILE_W=960;   MOBILE_Q=70

DURATION="$(ffprobe -v error -show_entries format=duration -of csv=p=0 "$SRC")"

export_set () {
  local name="$1" count="$2" width="$3" quality="$4"
  local dir="$OUT/$name"
  mkdir -p "$dir" "$TMP/$name"

  # pick the sample rate so the clip yields about <count> frames
  local fps
  fps="$(awk -v c="$count" -v d="$DURATION" 'BEGIN { printf "%.4f", c / d }')"

  ffmpeg -v error -i "$SRC" \
    -vf "fps=$fps,scale=$width:-2:flags=lanczos" \
    -frames:v "$count" -start_number 1 "$TMP/$name/frame_%04d.png"

  for png in "$TMP/$name"/frame_*.png; do
    base="$(basename "${png%.png}")"
    cwebp -quiet -q "$quality" -m 6 "$png" -o "$dir/$base.webp"
    avifenc -q "$((quality - 15))" -s 6 -j all "$png" "$dir/$base.avif" >/dev/null
  done
  echo "exported $name: $(ls "$dir"/*.webp | wc -l) frames"
}

export_set desktop "$DESKTOP_COUNT" "$DESKTOP_W" "$DESKTOP_Q"
export_set mobile  "$MOBILE_COUNT"  "$MOBILE_W"  "$MOBILE_Q"

# Poster and OG image from the last desktop frame
LAST="$(ls "$OUT"/desktop/frame_*.webp | tail -n 1)"
mkdir -p public/posters
cp "$LAST" public/posters/hero.webp
ffmpeg -v error -y -i "$LAST" \
  -vf "scale=1200:630:force_original_aspect_ratio=increase,crop=1200:630" \
  -q:v 3 public/posters/og.jpg

# 1 px AVIF used to detect decoder support
ffmpeg -v error -y -f lavfi -i color=black:s=2x2 -frames:v 1 "$TMP/probe.png"
avifenc "$TMP/probe.png" public/probe.avif >/dev/null
```
Frame counts can come out one short because of rounding. The audit records the real count (§5). That number, not the target, goes into the database.

## 5. Audit Script
```js
// scripts/audit-frames.mjs
// Usage: node scripts/audit-frames.mjs [version]
import { readdirSync, statSync, writeFileSync } from 'node:fs';
import { join } from 'node:path';

const version = process.argv[2] ?? '';
const root = join('public', 'frames', version);
const BUDGET = { desktop: 12 * 1024 * 1024, mobile: 5 * 1024 * 1024 };

let failed = false;
const manifest = {};

for (const device of ['desktop', 'mobile']) {
  const dir = join(root, device);
  for (const ext of ['webp', 'avif']) {
    const files = readdirSync(dir).filter((f) => f.endsWith(`.${ext}`)).sort();
    const bytes = files.reduce((sum, f) => sum + statSync(join(dir, f)).size, 0);
    const ok = files.length > 0 && bytes <= BUDGET[device];
    manifest[`${device}.${ext}`] = { count: files.length, bytes, ok };
    console.log(`${device}/${ext}: ${files.length} frames, ${(bytes / 1048576).toFixed(1)} MB ${ok ? 'OK' : 'FAIL'}`);
    if (!ok) failed = true;
  }
}

writeFileSync(join(root, 'manifest.json'), JSON.stringify(manifest, null, 2));
process.exit(failed ? 1 : 0);
```
`npm run frames -- <video> [version]` runs the export and then this audit.

If a set is over budget, change in this order: lower the quality (`*_Q`), then the width, then the frame count. Update the counts in the script and the PRD together.

## 6. Format Selection (AVIF with WebP fallback)
Both formats are always exported into the same folder, so the fallback works without extra configuration. `PageConfig.frames.*.ext` states the **preferred** format. The loader downgrades to WebP when the browser cannot decode AVIF:

```js
export async function supportsAvif() {
  try {
    const res = await fetch('/probe.avif');
    await createImageBitmap(await res.blob());   // rejects when AVIF is unsupported
    return true;
  } catch {
    return false;
  }
}
// frameUrl = `${basePath}/frame_${String(i).padStart(4, '0')}.${useAvif ? 'avif' : 'webp'}`
```
Until the Phase 6 optimization task ships, set `ext = 'webp'` and skip the probe.

## 7. Caching and Versioning
Frames are served with `Cache-Control: public, max-age=31536000, immutable` (see [Deployment Runbook](./08-DEPLOYMENT-RUNBOOK.md) §4). Because browsers will never re-fetch them:

- **Never overwrite** files that are already live.
- The first release may use the unversioned folder. Every later re-export **must** use a version folder (`v2`, `v3`, …).

## 8. Updating Frames
1. Export to a new folder: `npm run frames -- source/hero-v2.mp4 v2`.
2. Check the audit output and open a few frames (first, middle, last) on a phone-sized viewport.
3. Deploy the new files (they are inert until the database points at them).
4. Point the database at them, using the real counts from `public/frames/v2/manifest.json`:
```sql
update frame_sets set base_path = '/frames/v2/desktop', frame_count = 150, total_bytes = 11800000
 where device = 'desktop' and scene_id = '<scene-id>';
update frame_sets set base_path = '/frames/v2/mobile',  frame_count = 120, total_bytes = 4700000
 where device = 'mobile'  and scene_id = '<scene-id>';
update scenes set poster_url = '/posters/hero.webp' where id = '<scene-id>';
```
5. Wait for the page cache to expire (about 5 minutes), then check the live page.
6. Run `npm run export:content` so the static fallback matches.
7. Delete the old folder once the new set has been live for a week.

`total_bytes` is the size of the format served by default (WebP), taken from the manifest.

## 9. Troubleshooting
| Symptom | Cause | Fix |
|---|---|---|
| Fewer frames than the target | Rounding in `fps` | Use the manifest count in `frame_sets` |
| Over budget | Detailed or noisy footage | Lower quality, then width, then count |
| Banding in dark areas | Aggressive AVIF quantization | Raise AVIF quality by 5–10 |
| Mobile subject cropped | Subject outside the central 60% | Re-frame the source or crop in the script |
| New frames don't appear | Old path cached as immutable | Use a version folder and update `base_path` |
| `avifenc: unknown option -q` | Old libavif | Upgrade to 0.11+ |

## 10. Production Process (Upstream of the Pipeline)
This is how the source video gets made and approved before §4 runs.

| Step | Owner | Output | Approval |
|---|---|---|---|
| Brief | PM + Designer | Video brief: goal, length, tone, safe zones, no baked-in text (§2) | Stakeholder |
| Storyboard | Designer | Scroll storyboard (progress % → frame → copy), see [App Flow §5](./03-APP-FLOW.md) | Stakeholder |
| Production (shoot, 3D render or licensed footage) | Video vendor | Raw footage | PM |
| Edit and color | Vendor | Rough cut, then fine cut. **Limit review to 2 rounds** in the contract. | Designer + Stakeholder |
| Master export | Vendor | H.264 or ProRes master that meets §2 | Designer checks safe zones and that no text is baked in |
| Rights | PM | Written licenses for footage, music (none planned), people and locations; ownership of the master | Legal |
| Handoff | Vendor → Frontend | `source/hero.mp4` plus the license documents | Frontend runs the pipeline and the audit |

Rules:
- Placeholder footage used during the build must be replaced before launch. Check that its license also covers a public site.
- Keep the master and the licenses in a shared, access-controlled location outside the repo.
- A re-shoot or a new cut restarts at the storyboard step and goes live through the versioned process in §8.

---
