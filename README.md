# Vijayakumar Boopathy — Personal Site (Dark Glass, Single File)

This repo contains a **single-file** personal portfolio site (`index.html`) with a dark “glass” aesthetic, tabbed navigation, a persisted theme toggle, and interactive skill pills. It’s intentionally **no-build / no-dependencies**.

## Contents

- `index.html`: The entire site (HTML + inline CSS + inline JS).
- `resume.pdf`: A resume PDF asset.
- `HOW_TO_DO_IT.md`: A playbook for recreating/iterating on this single-file design.

## Run locally

You can open `index.html` directly in a browser, but using a tiny local server is recommended (better relative asset handling and fewer browser restrictions).

### Option A: Python

```bash
python3 -m http.server 8080
```

Then visit `http://localhost:8080`.

### Option B: Node

```bash
npx serve .
```

## Customize

### Update identity + links

Edit `index.html`:

- **Name/title/meta**: `<title>` and the `<meta name="description">`
- **Topbar brand**: `.brand`
- **About CTA links**: the button links (Email / LinkedIn / GitHub / LeetCode / Resume)
- **Work timeline**: section `#work`
- **Projects**: section `#projects`
- **Contact grid**: section `#contact`

### Change theme colors

In the `<style>` block, tweak the CSS variables in `:root` (dark) and `[data-theme="light"]` (light), e.g. `--accent`, `--accent2`, `--panel`, `--bg-grad`.

### Skill pills (tooltips + ratings)

Skills are rendered as pills like:

```html
<span class="pill skill" tabindex="0" data-skill="Java" data-rating="5">Java</span>
```

- `data-rating` is `1..5` (used to render stars + a level label)
- Tooltip is created by the inline `<script>`

## Deploy

This is a static site. Common options:

- **GitHub Pages**: put `index.html` at the repo root, enable Pages for the main branch.
- **Netlify/Vercel/Cloudflare Pages**: deploy as a static site, build command: *(none)*, output folder: *(root)*.

## Notes

- **Theme**: toggles `data-theme="light"` on `<html>` and persists in `localStorage` key `theme`.
- **Tabs**: switch panels by toggling `.active` on `.tab` buttons and `.panel` sections.
- **Reduced motion**: honors `prefers-reduced-motion`.

