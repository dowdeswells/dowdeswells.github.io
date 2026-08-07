# dowdeswells.github.io

**Software Development Thoughts** — my GitHub Pages site, dressed up as a retro arcade cabinet.

## 🕹️ Retro arcade themes

The site ships with three arcade skins, switched with one line in `_config.yml`:

| `arcade_theme` | The look |
| -------------- | -------- |
| `neon`         | **80s neon** — hot pink & cyan glow on deep purple (default) |
| `phosphor`     | **Amber CRT** — monochrome phosphor terminal, heavy scanlines |
| `vector`       | **Vector cabinet** — cyan wireframe lines on black (Asteroids-style) |

To switch, set e.g. `arcade_theme: phosphor` and push — GitHub Pages rebuilds automatically.

Fonts are loaded from Google Fonts: **Press Start 2P** (pixel headings) and **VT323** (CRT body text).

Screenshots of each theme live in [`preview/`](preview/) (excluded from the published site).

## 🗂️ Where things live

- `_layouts/` — `default`, `home` and `post` layouts (arcade-styled)
- `_includes/head.html` — loads the Google Fonts + `assets/css/arcade.css`
- `assets/css/arcade.css` — all the arcade styling; each theme is a block of CSS variables
- `index.md` — the homepage (uses the `home` layout)
- `_posts/` — your posts, untouched

Note: `README.md` is excluded from the published site
(`exclude: [README.md]` in `_config.yml`) so it only exists as repo documentation.
