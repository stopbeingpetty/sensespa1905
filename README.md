# Senses Spa — Website

Premium booking website for Senses Spa at Hotel Navis, Opatija.
Single-file HTML + responsive image set.

---

## File structure

```
senses/
├── index.html          ← the entire site (open in browser to preview locally)
└── images/             ← 25 responsive WebP files (4 sizes per photo)
```

Just double-click `index.html` to preview — works offline, no build step.

---

## Deploying to GitHub Pages (recommended)

1. **Create a new repo** on GitHub. Name it whatever — `senses-spa` works.
2. **Upload the contents** of this folder (not the folder itself — drag `index.html` and the `images/` folder directly into the repo).
3. **Enable Pages**: repo Settings → Pages → Source: `main` branch, root folder. Save.
4. Wait ~30 seconds. Your site is live at `https://YOUR-USERNAME.github.io/senses-spa/`.
5. **Custom domain (optional)**: in the same Pages settings, add `www.senses-spa.com` or similar. Point your domain's CNAME to `YOUR-USERNAME.github.io`. Done.

### Updating
- Edit any file (HTML or swap an image) → commit → push → live in 30 seconds.
- For local edits before pushing, the file path inside index.html stays the same (`images/team-lana-800.webp` etc.) so nothing breaks.

---

## Replacing an image later

Each photo has 3–4 size variants in WebP. To swap a photo:

1. **Process new image** to multiple widths. Run this Python snippet:
   ```python
   from PIL import Image
   img = Image.open('new-photo.jpg').convert('RGB')
   for w in [1200, 800, 480]:
       h = int(img.height * w / img.width)
       img.resize((w, h), Image.LANCZOS).save(f'team-lana-{w}.webp', 'WEBP', quality=82, method=6)
   ```
2. **Replace files** in `images/` keeping the exact same filenames.
3. Push. Done.

No HTML edits needed — paths stay the same.

---

## Image map (which photo goes where)

| Location | File prefix | Source photo |
|---|---|---|
| Hero (fullscreen) | `hero-facial` | Facial gua sha treatment |
| Philosophy main | `philosophy` | Pharmos Natur + jade gua sha + aloe |
| Philosophy accent | `sanctuary-treatment` | Body gua sha on leg |
| Story intro | `team-group` | Marijana, Lana, Iris with brochure |
| Team — Marijana | `team-marijana` | Cropped from group photo |
| Team — Lana | `team-lana` | Solo at pool |
| Team — Iris | `team-iris` | Solo at pool |
| Visit | `visit-interior` | Spa Navis entrance |

**Still placeholders (no photo yet):**
- Sanctuary section: Sauna card + Pool card — these still show the elegant placeholder. When you get sauna/pool photos, drop them in as `sauna-800.webp`, `sauna-1200.webp` etc. and update the two `<div class="img-slot" data-image-slot="sauna">` blocks the same way other photos are wired.

---

## Configuration to remember

- **Default language:** English (EN/HR/DE/IT switcher in topbar)
- **Booking email:** `hotel@hotel-navis.hr` (constant `SPA_EMAIL` near bottom of `<script>` in `index.html`)
- **Google Calendar integration:** Currently uses mailto + GCal quick-add link. To upgrade to direct Google Calendar event creation, see commented Apps Script skeleton at the bottom of `<script>` in `index.html`.

---

## Technical notes

- All images served as WebP (~5% of original file size, no visible quality loss)
- `<picture>` + `srcset` automatically serves correct resolution per device:
  - Mobile gets 480w/800w
  - Desktop gets 1200w/1600w
  - Retina desktop gets 2400w (hero only)
- `loading="lazy"` on all non-hero images for fast initial paint
- Hero uses `loading="eager" fetchpriority="high"` for LCP optimization
- Total image payload: ~1.7 MB across 25 variants
