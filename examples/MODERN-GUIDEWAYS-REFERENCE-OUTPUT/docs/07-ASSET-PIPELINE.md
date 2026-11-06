# 07 — Asset Pipeline (Existing Frames)
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Current project status
**Video generation and frame splitting are already completed by the project owner.** The supplied desktop screenshot shows a `frames/` folder and sequential WebP files such as `frame_0001.webp`, `frame_0002.webp`, `frame_0003.webp` and onward. The full directory was not supplied as an upload, so the exact count, dimensions, total payload, numbering gaps and desktop/mobile variants are still unverified.

**Do not repeat video generation or frame extraction as a required project step.** Begin with inventory and validation of the current frame sequence.

## 2. Intake procedure
1. Copy the existing frame folder into a versioned working directory; keep the original untouched.
2. Confirm filename pattern and sort numerically, not alphabetically.
3. Count files and detect gaps, duplicates, mixed extensions and unreadable images.
4. Record dimensions, bytes per frame and total bytes.
5. Generate a contact sheet at regular intervals and inspect the first, middle, last and all scene transitions.
6. Confirm whether the sequence is desktop landscape, portrait or a single shared crop.
7. Only then create the production manifest and storyboard mapping.

## 3. Recommended production paths
```text
public/
  frames/
    v1/
      desktop/
        frame_0001.webp
        frame_0002.webp
        ...
      mobile/                 # optimized derivatives from existing frames if needed
        frame_0001.webp
        frame_0002.webp
        ...
      manifest.json
  posters/
    hero.webp
    og.jpg
```

If only one frame set exists initially, keep it under `v1/source-set/` or `v1/desktop/` until viewport behavior is approved. Do not label it “mobile” without a separate measured crop/test.

## 4. File naming and sequence validation
Accepted initial pattern: `frame_0001.webp`. The index width must be consistent. The audit should reject duplicates and gaps; do not silently renumber unless the owner approves the new canonical order. Maintain a mapping file if source numbering must be translated.

## 5. Manifest example
```json
{
  "version": "v1",
  "filenamePattern": "frame_{index}.webp",
  "indexStart": 1,
  "desktop": {"path":"/frames/v1/desktop/", "count": null, "width": null, "height": null, "totalBytes": null},
  "mobile": {"path": null, "count": null, "width": null, "height": null, "totalBytes": null},
  "poster": "/posters/hero.webp",
  "auditStatus": "pending"
}
```

Replace every `null` with measured values before integrating the renderer. The manifest template is also provided under `templates/frame-manifest.example.json`.

## 6. Image optimization
- Preserve an untouched source copy.
- Produce WebP and optionally AVIF derivatives from existing frame images; do not recompress a lossy derivative repeatedly.
- Tune quality by inspecting fine metal edges, dark bellows, reflections and gradients for artifacts.
- Set a consistent maximum width for each target viewport.
- Create mobile crops from the same original sequence only after checking that the moving machine/cover remains visible.
- Use `hero.webp` as a poster for no-JS and reduced-motion states.

## 7. Initial budget
Use the source plan's provisional ceilings as the first audit target: ≤12 MB for the selected desktop frame set and ≤5 MB for the selected mobile set. The actual acceptable total depends on the measured sequence and device tests. If the sequence exceeds budget, optimize in this order: quality, resolution, frame count, then consider splitting into shorter scenes. Do not remove frames until visual smoothness is reviewed.

## 8. Loading strategy
Load keyframes first (for example every fourth index), then intermediate frames (every second index), then all remaining frames. Keep the poster visible while loading. A timeout must reveal a Continue option. Draw the nearest decoded frame if the exact requested frame is not ready.

## 9. Storyboard and frame audit
Create a contact sheet and complete `templates/scroll-storyboard.csv`. Record frame ranges, visible subject, approved overlay copy, overlay safe area, mobile crop concerns and reviewer sign-off. Do not assume that a product is visible just because it is named in the overlay.

## 10. Versioning and release
Never overwrite live frame URLs because CDN caches may be immutable. Publish a new directory such as `v2/`, update the manifest and page configuration, smoke-test, then retain the previous set for rollback. Delete old frame sets only after the approved retention window.

## 11. Troubleshooting
| Symptom | Likely cause | First check |
|---|---|---|
| Frame order jumps | Alphabetic sorting or numbering gaps | Numeric sort + audit report |
| Blank frame | 404 or decode error | Network tab and image decoder |
| Subject cropped on phone | Desktop-only crop | Build/test mobile derivative |
| Scrub feels delayed | Too many frames decoded on main thread | Decode to ImageBitmap and profile |
| New frames do not appear | Immutable cached URL | Publish new version path |
