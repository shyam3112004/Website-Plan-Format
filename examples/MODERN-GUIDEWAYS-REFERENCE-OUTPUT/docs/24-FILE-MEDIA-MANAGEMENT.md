# 24 — File & Media Management
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Media inventory
- MG logo: obtain clean transparent SVG/PNG master and variants.
- Scroll frames: existing `frame_0001.webp`-style sequence; exact count to be audited.
- Hero poster: selected still from approved frame sequence.
- Product photos: approved images matching each category.
- Workshop/machine photos: genuine company images only.
- Social preview image: 1200×630 recommended export.
- Icons and favicon set.

## 2. Folder convention
```text
public/
  brand/
  frames/v1/desktop/
  frames/v1/mobile/
  posters/
  products/
  gallery/
  icons/
```

Keep originals and license evidence outside the public web root. Public paths should contain only approved, optimized exports.

## 3. Upload/processing checks
Validate extension and actual file type, dimensions, file size, transparency, color profile and filename. Strip unnecessary metadata from public images. Generate responsive sizes for large still images.

## 4. Rights and consent
Confirm that all people, workshop interiors, customer machines, logos and third-party materials may be published. Obtain permission where necessary. Do not expose customer job labels, drawings or serial numbers accidentally.

## 5. Versioning and rollback
Never overwrite live animation frame URLs. Use immutable version folders and maintain a manifest. For gallery changes, use version-controlled filenames or cache revalidation. Keep backups of approved originals.

## 6. Accessibility
Write informative alt text for products and informative workshop images. Decorative geometric accents use empty alt or CSS backgrounds.
