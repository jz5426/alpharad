# AlphaRad — project page

A single-file project page for *AlphaRad: Grounded Zero-Shot Classification in Chest Radiology
via α-Corrected Binary Cross Entropy and Factorized Latent Supervision.*

No build step, no dependencies to install. Drop it in a repo and turn on Pages.

```
index.html          the whole page — markup, styles, data and behaviour
assets/*.png        placeholder figures, ready to be overwritten
.nojekyll           stops GitHub Pages from running Jekyll over the folder
```

## Publish it

1. Create a repository named `<your-username>.github.io` (for a user site) or any repo (for a project site).
2. Copy `index.html`, `assets/` and `.nojekyll` into the root and push to `main`.
3. **Settings → Pages → Build and deployment → Source: Deploy from a branch**, branch `main`, folder `/ (root)`.
4. The page appears at `https://<your-username>.github.io/` or `https://<your-username>.github.io/<repo>/`.

To preview locally: `python3 -m http.server 8000` then open `http://localhost:8000`.

## Drop in your figures

Every image is a static placeholder holding the slot for one figure from the paper — whole figures,
exactly as they appear in the PDF, nothing cropped or sliced. Each placeholder is labelled with the
figure number, what it shows, and the size it was drawn at.

Export the figure, overwrite the file, keep the name. Nothing else to change.

| File | Figure | Where it appears |
|---|---|---|
| `fig03-similarity-maps.png` | 3 — similarity maps on ChestXDet10 | Hero, under the title |
| `fig01-fusion-modules.png` | 1 — types of cross-modal fusion | Method → Architecture |
| `fig02-framework.png` | 2 — overall training framework | Method → Architecture |
| `fig13-grounding-success.png` | 13 — grounding, success cases | Qualitative |
| `fig14-grounding-failure.png` | 14 — grounding, failure cases | Qualitative |
| `fig15-segmentation-chexlocalize-a.png` | 15 — CheXlocalize, classes 1–5 | Qualitative |
| `fig16-segmentation-chexlocalize-b.png` | 16 — CheXlocalize, classes 6–10 | Qualitative |
| `fig06-segmentation-siim.png` | 6 — SIIM-ACR pneumothorax | Qualitative |
| `fig07-segmentation-qata.png` | 7 — QaTa-COV19 | Qualitative |
| `fig05-factorized-subspaces.png` | 5 — the eight FLaS subspaces | Qualitative |

Notes:

- **Any size works.** Images scale to the width of their container, so the placeholder dimensions are a
  suggestion, not a requirement. Aim for roughly twice the display width — the tall contact sheets show
  at about 500 px on a desktop, the wide diagrams at about 500 px in a pair or 1100 px full width.
- **Different extension?** Change the `src` on the matching `<img>` tag. That's the only reference.
- **Don't need a figure?** Delete its whole `<figure class="figure">…</figure>` block; nothing else
  depends on it.
- The two figures paired inside a `.figgrid` sit side by side above 900 px and stack below it, so pairs
  read best when the two images have similar proportions.
- The FLaS diagram in the method section is drawn in inline SVG, not an image. If you'd rather use
  Figure 1 there, the slot under **Architecture** already holds it.

## Fill in before publishing

Everything below is marked with an HTML comment in `index.html`.

| What | Where | Currently |
|---|---|---|
| Paper, Code, Checkpoints links | `.actions` in the hero | `href="#"` |
| Author homepages | `.authors` — swap each `<span>` for an `<a href="…">` | plain text |
| `og:image` URL | `<head>` | relative path; use an absolute URL for link previews to render |
| BibTeX entry | `#cite` | arXiv id is a placeholder |
| Venue | hero eyebrow | says `Preprint` |

## Theme

The page ships in U of T colours and carries a dark/light toggle in the top bar.

- **Dark** — U of T Blue `#002A5C` is the panel surface, sitting on a deeper navy ground.
- **Light** — the same navy becomes the type, on white panels over a pale blue ground.

First visit follows the visitor's OS setting; once they use the toggle, their choice is remembered in
`localStorage` and the OS is no longer followed. A small inline script in `<head>` resolves the theme
before first paint, so there's no flash of the wrong theme. If storage is blocked, the toggle still
works for the session.

Colour is doing semantic work throughout and is worth preserving if you restyle:

| Token | Means |
|---|---|
| `--gt` | radiologist ground truth |
| `--pred` | model prediction |
| `--peak` | attention peak, and best-in-column in the tables |

Each has an `-ink` variant used wherever the signal appears as type. The fills stay bright in both
themes so the legend swatches match the radiographs; the ink variants darken in light mode to stay
legible on white. All of it lives in the `:root` and `:root[data-theme="light"]` blocks at the top of
the stylesheet.

## Where the numbers come from

Four tables are generated from plain JS arrays near the top of the `<script>` block, all transcribed
from the paper:

| Array | Paper table |
|---|---|
| `CLS_ROWS` | Table 2 — zero-shot classification, 16 datasets |
| `DENSE_ROWS` | Table 3 — grounding, phrase grounding, segmentation |
| `CXD_ROWS` | Table 4 — per-disease Pointing Game on ChestXDet10 |
| `CXL_ROWS` | Table 12 — per-disease Dice on CheXlocalize |
| `HEADS` | Tables 10 & 11 — per-head results, shown as the dot plot in the FLaS panel |

Edit a number there and the table and its best-per-column highlighting both update. A row's fourth
element flags it: `'ours'` marks it as an AlphaRad row, `'rule'` draws a divider under it.

To add another table, give it an empty `<table id="…">` and one `buildTable()` call.

## Notes

- Fonts come from Google Fonts and equations from the KaTeX CDN. To vendor them, download both into
  `assets/` and repoint the three `<link>`/`<script>` tags. Equations fall back to readable plain text
  if KaTeX fails to load.
- No animation, no interactive image controls — every figure is static. Responsive to 360 px,
  keyboard-navigable, and `prefers-reduced-motion` is respected.
