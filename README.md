# Mentor Bridge · Session Slides

A multi-deck [Slidev](https://sli.dev/) workspace for Mentor Bridge engineering sessions. Browse every topic from a homepage; open a topic to present it fullscreen in the browser.

## Current sessions

| Topic | Slug | Run |
| --- | --- | --- |
| Before You Join IT: Build Your Professional Toolset | `before-you-join-it` | `pnpm dev before-you-join-it` |
| SOLID with React | `solid-with-react` | `pnpm dev solid-with-react` |

## Quick start

```bash
pnpm install

# Homepage listing all topics (http://localhost:3000)
pnpm dev

# Work on one presentation (http://localhost:3030/?present=1)
pnpm dev git-fundamentals

# Scaffold a new deck
pnpm new react-hooks "React Hooks Deep Dive"

# Production-like browse: homepage + every deck under one server
pnpm build
pnpm preview   # http://localhost:4173 — click a topic to present
```

Requires **Node.js ≥ 22.12** and [pnpm](https://pnpm.io/) (`npm i -g pnpm`).

During `pnpm dev` (homepage only), clicking a topic prompts you to run `pnpm dev <slug>` in another terminal. After `pnpm build && pnpm preview` (or a deploy), clicking a topic opens that deck with `?present=1` and requests fullscreen.

## Repository layout

```text
presentations/          # one folder per topic
  git-fundamentals/
    slides.md           # Slidev entry (frontmatter = catalog metadata)
  …
homepage/               # topic index (search + category filters)
scripts/                # catalog, build, dev, new, export
components/             # shared Vue components for decks
global-bottom.vue       # auto-fullscreen when ?present=1
```

## Adding a session

1. `pnpm new <kebab-slug> "Title"`
2. Edit `presentations/<slug>/slides.md`
3. Fill catalog fields in the headmatter:

```yaml
---
theme: default
title: My Session
info: |
  One or two sentences shown on the homepage.
category: technical          # or: non-technical
tags: [api, http]
duration: 45m
level: beginner              # beginner | intermediate | advanced
draft: false                 # true hides from production build
---
```

4. Run `pnpm dev <slug>` while editing, or refresh the homepage (`pnpm dev`) to see it listed.

## Presenting

- From the homepage, click a topic → opens `/{slug}/?present=1` and requests fullscreen.
- If the browser blocks fullscreen, press **`f`** (Slidev) or **`F11`**.
- Arrow keys / Space navigate; see [Slidev UI](https://sli.dev/guide/ui) for presenter view (`p`).

## Deploy

### Vercel (recommended)

Project is linked under the **mentorbridges-projects** team.

```bash
# Redeploy production
vercel deploy --prod --yes --scope mentorbridges-projects
```

Production URL: https://session-slides-xi.vercel.app

To auto-deploy on every git push (after the first commit + push to GitHub):

```bash
vercel git connect --scope mentorbridges-projects
```

`pnpm build` outputs:

- `dist/index.html` — homepage
- `dist/<slug>/` — each presentation SPA

Configs for **Vercel** (`vercel.json`) and **Netlify** (`netlify.toml`) are included. For GitHub Pages, set `BASE_PATH=/session-slides` during build.

## Scripts

| Command | Purpose |
| --- | --- |
| `pnpm dev` | Homepage on `:3000` |
| `pnpm dev <slug>` | Slidev for one deck on `:3030` |
| `pnpm catalog` | Regenerate `homepage/catalog.json` |
| `pnpm build` | Static build of homepage + all non-draft decks |
| `pnpm new <slug> [title]` | Scaffold a deck |
| `pnpm export <slug> [pdf\|png\|pptx]` | Export a deck |

## Learn Slidev

- [Getting started](https://sli.dev/guide/)
- [Syntax guide](https://sli.dev/guide/syntax)
- [Hosting](https://sli.dev/guide/hosting)
