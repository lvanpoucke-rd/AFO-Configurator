# AFO-Configurator

Kids AFO Configurator — a single, responsive page (`index.html`) that works
on both desktop and mobile, no separate mobile link required
(`mobile.html` just redirects to `index.html` for old links).

`index.html` is fully self-contained: every image, the vendored jsPDF
library, and the title font are embedded directly in the file as data
URIs. You can open it straight from disk (double-click it, or drag it
into a browser tab) with nothing else alongside it, or serve it from any
static file server / GitHub Pages — both work identically. The `assets/`
folder holds the original source files (kept for editing/maintenance);
it is not required at runtime.

## How it's built

- Plain HTML/CSS/JS, no build step.
- The AFO render is a single `<canvas>`. Five pixel-aligned layer images
  (outer shell, inner shell, straps, laces, silver accents; originals in
  `assets/layers/`) are drawn back to front; the silver accents layer is
  always drawn last so metal parts are never tinted.
- Colors and patterns are applied with a canvas `multiply` blend against
  each layer's own artwork, so the original shading/highlights always
  show through.
- Transfer paper patterns, leather/microfiber colors and lace colors are
  the same data as before (same amount, order, names and number codes) —
  only the layout changed. Lace swatches show a plain color circle
  (sampled once from the product photo) rather than the photo itself.
- The **Save** button renders the current configuration (a downscaled
  copy of the canvas + the selections) to a PDF via a vendored jsPDF
  build, entirely client-side.

## Updating an asset

If you need to swap an image, replace the file under `assets/`, then
regenerate the matching data URI in `index.html` (base64-encode the file
and paste it in place of the existing one — see the `LAYER_SRC` object,
the `<img>` tags, and the `@font-face` rule).

## Fonts

- Body text uses **Noto Sans**, loaded from Google Fonts (falls back to
  the system sans-serif if offline).
- The title ("Customize your AFO.") uses the licensed **Bogue Semibold
  Italic**, embedded in the `@font-face` rule in `index.html`.
