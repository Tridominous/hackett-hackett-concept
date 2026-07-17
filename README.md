# Hackett &amp; Hackett — Homepage Concept (v2, photographic)

A reimagined homepage concept for **Hackett &amp; Hackett** (luxury chauffeur, close
protection &amp; private travel). This is **v2**: the same design system as v1, now carrying
real photography. `../hackett-homepage/` is left untouched as the illustrative original.

> Independent interview / portfolio concept — not affiliated with or endorsed by Hackett &amp; Hackett.

---

## What's in here

```
hackett-homepage-v2/
├── index.html     ← the entire site (HTML + CSS + JS inline, inline SVG art)
├── assets/        ← 9 photographs, WebP, ~1.2 MB total
├── .nojekyll      ← tells GitHub Pages to serve files as-is
└── README.md      ← you are here
```

Still no build step, no framework, no npm install. One HTML file plus an image folder.

---

## What changed from v1

- **Photography replaces the line art.** A full-bleed hero plate, five fleet plates, a
  close-protection portrait, a feature-tile backdrop and a London-night closing plate.
- **Treatment is tokenised, not baked in.** Opacity, grade (`saturate/contrast/brightness`)
  and every scrim live in CSS custom properties, so both themes re-grade the same files.
  Night photography can't just be dimmed onto porcelain — it turns grey — so light mode
  lifts and desaturates it into a ghosted plate instead.
- **Three latent layout bugs fixed** (all present in v1, invisible until real images and
  real screenshots put weight on the layout):
  - `.band__in` used the `padding` shorthand while also being a `.wrap`, silently zeroing
    the page gutter and letting the protection band bleed to the viewport edge.
  - `.tile > *` was declared *after* `.tile__tag` / `.tile__bg` at equal specificity,
    resetting them to `position: relative` — the "SIA Licensed" pill rendered as a
    full-width bar and the tile backdrop stopped bleeding.
  - `.fcard__price` wrapped "On request" onto two lines and crowded the seat count.
- **Mobile**: the hero scrim switches from a side-ramp to a vertical cover (copy spans the
  full width there), and the wordmark no longer wraps beside the theme toggle.

---

## Photography

Nine images, sourced from [Unsplash](https://unsplash.com/license) and committed locally to
`assets/` — nothing is hotlinked, so the page stays self-contained and offline-safe.

| File | Used for |
|---|---|
| `hero-sclass-night.webp` | Hero — black S-Class, wet street, night |
| `fleet-eclass.webp` | Fleet — Business Class |
| `fleet-sclass.webp` | Fleet — First Class |
| `fleet-vclass.webp` | Fleet — MPV / Group |
| `fleet-rangerover.webp` | Fleet — SUV Class |
| `fleet-rolls.webp` | Fleet — Elite Class |
| `protection-officer.webp` | Close-protection band |
| `cta-dusk.webp` | Services — feature tile backdrop |
| `london-night.webp` | Final CTA backdrop |

**Licensing note.** The Unsplash licence allows free commercial and non-commercial use
without permission or attribution. Credit is given in the page footer anyway, which is the
honest position for a concept piece. These are **placeholder art direction** — they stand in
for a real shoot and are not Hackett &amp; Hackett's own vehicles or staff. Swap them for
client photography before anything client-facing; drop a file with the same name into
`assets/` and nothing else needs to change.

**Performance.** Hero is preloaded (`fetchpriority="high"`); everything below the fold is
`loading="lazy"`. Every `<img>` carries `width`/`height` so nothing shifts as plates arrive.
If a file is ever missing, its frame reads as a dark well — the layout never breaks.

---

## Preview locally

Any static server works. From inside `hackett-homepage-v2/`:

```bash
npx --yes serve .          # → http://localhost:3000
python -m http.server 8000 # if you have a real Python (see note)
```

> Note: on this machine `python` is a Microsoft Store alias stub, not an interpreter, so
> `python -m http.server` fails — and `.claude/launch.json` in the parent folder is
> configured to use it. Use the `npx serve` line above, or install Python from python.org.

---

## Deploy: GitHub Pages

1. Create a new GitHub repo (e.g. `hackett-homepage`).
2. Put the **contents** of this folder (`index.html`, `assets/`, `.nojekyll`) at the repo
   root and push:
   ```bash
   git init
   git add .
   git commit -m "Hackett & Hackett homepage concept"
   git branch -M main
   git remote add origin https://github.com/<you>/hackett-homepage.git
   git push -u origin main
   ```
3. Repo → **Settings → Pages** → *Build and deployment* → **Deploy from a branch** →
   Branch: `main`, folder: `/ (root)` → **Save**.
4. Live in ~1 minute at `https://<you>.github.io/hackett-homepage/`.

> Keep `assets/` next to `index.html` — the image paths are relative.
> Keeping this inside a larger repo? Pages only serves from the repo **root** or a
> **`/docs`** folder — rename this folder to `docs/` and pick `/docs` in step 3.

---

## Deploy: Vercel

- **No-git option:** [vercel.com/new](https://vercel.com/new) → drag this folder on → Deploy.
- **CLI option:** from inside this folder:
  ```bash
  npm i -g vercel
  vercel        # accept defaults; Framework Preset = "Other"
  vercel --prod # promote to production
  ```
- **Git option:** import the repo on Vercel; no configuration needed for a static site.

---

## Customising

Everything is driven by CSS custom properties near the top of `index.html` (`:root { … }`),
with the light theme overriding the same names under `:root[data-theme="light"]`:

- **Brand/surface:** `--brass`, `--void`, `--ivory`, fonts, `--r` (radius).
- **Photography:** `--ph-hero-op`, `--ph-hero-grade`, `--ph-grade`, `--ph-tile-op`,
  `--ph-cta-op`, and the `--scrim-*` ramps. Tune imagery here, not in the rules.
- **Copy and rates:** plain HTML. The indicative rates (`£75`, `£100`) live in the markup
  and in the quote card's `data-rate` attributes.
