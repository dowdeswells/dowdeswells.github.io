---
name: style-iteration
description: Iterate on the retro arcade theme of this GitHub Pages (Jekyll) site, styling only. Use when asked to restyle, tweak, or enhance the arcade look.
---

# Arcade Style Iteration

## Hard Rule — Never Edit Content

- **`_posts/*.md` are OFF LIMITS. Do not edit, reword, add, remove, or reformat their content under any circumstances.** This includes front matter, titles, headings, prose, links, and images.
- Content changes require explicit user approval first — and even then, default to "no".

## Style Scope — Files You May Touch

- `assets/css/arcade.css` — main theme (three skins: `.theme-neon`, `.theme-phosphor`, `.theme-vector`)
- `_layouts/default.html`, `_layouts/home.html`, `_layouts/post.html` — markup/structure (allowed only if a change is purely presentational)
- `_includes/head.html` — style-related includes only
- `_config.yml` — only the `arcade_theme:` switch (neon | phosphor | vector) and cosmetic settings
- `index.html`, `index.md` — only if presentational; otherwise treat as content

## Workflow

1. Read `assets/css/arcade.css` fully before changing anything; respect its existing CSS-variable palette and three-skin structure.
2. Keep changes scoped: theme skin → shared variables → layout/component rules → markup.
3. Preserve responsive behavior, readability, and the "arcade" character of the site.
4. Verify locally before finishing:
   ```bash
   docker compose up   # or: bundle exec jekyll serve
   ```
5. Diff your changes and confirm no file under `_posts/` appears in the diff. Run:
   ```bash
   git diff --stat -- _posts/
   ```
   It must show nothing. If it shows anything, revert immediately.

## Done Checklist

- [ ] Only style files changed
- [ ] `git diff --stat -- _posts/` is empty
- [ ] Site builds and previews correctly
