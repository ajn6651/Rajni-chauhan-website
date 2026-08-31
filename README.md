# Rajnikant C. Chauhan — Personal Website

Single-page portfolio for Rajnikant C. Chauhan — digital textile designer, fine artist and sculptor (Surat, India). Pure HTML/CSS/JS, no build step.

## Structure

```
index.html      ← the website (deploy this)
assets/         ← optimised images (≈4 MB total) + favicon
assets/full/    ← high-res versions (≈2000px) the lightbox opens on click — loaded on demand only
sources/        ← original full-resolution photos & earlier HTML drafts (not needed for the site)
```

## Run locally

Open `index.html` directly in a browser, or serve the folder:

```sh
python3 -m http.server 8000
# → http://localhost:8000
```

## Deploy to Netlify

**Option A — drag & drop:** drag the whole folder (or just `index.html` + `assets/`) onto app.netlify.com/drop.

**Option B — Git:** push this folder to a GitHub repo, then in Netlify choose *Add new site → Import an existing project*. No build command needed; publish directory = repo root.

> Note: everything in the repo is published, including `sources/` (full-resolution originals). If you don't want those public, move `sources/` out of the folder before pushing.

## Notes on choices made

- **Brand mark & favicon:** the mark is a *registration frame + bindu*: a solid frame with corner crop ticks (a print/proofing reference) around a gold centre dot. `assets/favicon.svg` is the bold tab-size version, `favicon-32.png` is the Safari fallback and `apple-touch-icon.png` (180px) the iOS bookmark icon. In the nav/footer the same mark is an inline SVG: the frame follows `currentColor` (gold over dark scenes, indigo over the paper sections) while the bindu stays gold. The nav hierarchy deliberately favours the name: the wordmark is 19px Fraunces, the mark a small 28px accent (24px/17px on phones).
- **Icons / install:** `site.webmanifest` ships `icon-192.png` + `icon-512.png` (purpose `any`) and `maskable-icon-512.png` (purpose `maskable`). The maskable variant is full-bleed navy with the mark scaled to sit inside the circular safe zone, so Android launcher masks never crop the corner ticks.
- **Theme transitions:** dark (indigo) sections no longer hard-cut against the ivory ones — each fades in/out over ~120px via background gradients ("theme seams"). The scrolled nav adapts to the section beneath it: deep glass over dark sections, ivory glass over light ones, and the `theme-color` meta (mobile browser tint) swaps with it. Sections opt in with `data-theme="dark"`. A 2px gold **scroll-progress thread** fills along the nav's bottom edge (via `transform: scaleX`, updated in the existing scroll handler); it hides itself while the mobile menu is open.
- **Hero image:** the site now uses `assets/hero.jpg` (the high-resolution painting from `sources/hero-new - Copy.jpg`), replacing the low-res photo previously used. To revert, swap the hero `<img>` src back to `assets/guest-1.jpg`.
- **London feature section:** the two broken image slots (`london-feature-1.jpg`, `london-feature-2-h.jpg` — never present) now hold the featured painting (`assets/solo-artwork-1.jpg`, from `8556.JPG`) and the exhibition view (`assets/solo-artwork-2.jpg`, from `about-1 - Copy.jpg`).
  - Note: `about-1 - Copy.jpg` carries a broken EXIF rotation tag — macOS Preview shows it as a portrait, but the actual pixels are a landscape shot (1600×1126). The site uses the true landscape image and `solo-artwork-2.jpg` was re-encoded with ffmpeg to strip the misleading metadata.
- **Exhibitions list:** "2001 · Rotary Art Gallery, Surat" appears twice on purpose — the page claims ten one-man shows and lists exactly ten. If the duplicate is unintentional, delete one row in `index.html` (search for "Rotary Art Gallery").
- **Lightbox:** every content image (14 of them — profile, featured artwork, London gallery, sculpture, Raza, Modi letter, solo shows, Square Art, press clipping) is clickable and opens enlarged in a dark overlay. Images tagged `data-full` open a high-res file from `assets/full/` (re-encoded from `sources/` at ~2000px, quality q3) when one exists; the five photos whose page asset is already full resolution open the asset itself. The overlay loads the cached page image instantly, swaps in the full-size file when ready, pre-warms the neighbours, and supports ←/→ keys, swipe on touch screens, Esc / ✕ / backdrop-click to close, with body scroll locked while open.
- **Responsive behaviour:** the layout is verified at 360 / 390 / 414 / 768 / 1100 / 1280 px with zero horizontal overflow. Breakpoints:
  - **≤1250px** — desktop nav links swap to a hamburger (the nine links need ~1250px; below that they would collide with the brand).
  - **≤1080px** — all two-column feature grids and the three-column journey grid collapse to one column; gallery grid drops to 2 columns.
  - **≤760px** — hamburger menu (full-screen ivory panel; the nav turns ink-dark so the close button stays visible, background scroll locks), news cards stack, exhibition rows drop the "Solo" tag column.
  - **≤560px** — tighter 20px gutters, nav tag chips may wrap to two lines (a long nowrap tag otherwise overflows a 360px screen), gallery tiles tighten.
  - The hero uses `100svh` (small viewport height) so iOS Safari's address bar doesn't push the thesis text below the fold, and anchored sections carry `scroll-margin-top` so they never land under the fixed nav.
