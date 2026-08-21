# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Note: `CLAUDE.md` is a symlink to `AGENTS.md` — edit `AGENTS.md`.

## Project Overview

Diego Muñoz Zaldivar's personal portfolio — a single-page Astro 7 static site (no framework integrations or adapters — `astro.config.mjs` sets `site` and nothing else). Requires Node >= 22.12.0.

The reference design lives in `.resources/Portfolio.html` (a Claude design export); the site is a clean Astro port of it. Keep the code minimal: content is data arrays in page frontmatter, styling is plain CSS with custom properties, and client JS is limited to the theme toggle.

## Commands

```bash
npm run dev       # Start dev server (see background mode below)
npm run build     # Production build to dist/
npm run preview   # Build, then serve dist/ through Wrangler exactly as Cloudflare does
npm run astro     # Run the Astro CLI directly (e.g. npm run astro -- check)
npm run deploy    # Build, then wrangler deploy
```

There are no test or lint scripts configured.

### Dev server: use background mode

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

## Architecture

Standard Astro project layout:

- `src/pages/index.astro` — the whole site: content data (projects, background) in frontmatter, markup rendered statically, plus a pre-paint inline theme script and the toggle handler
- `src/styles/global.css` — all styling; theme is light/dark via CSS custom properties keyed off `data-theme` on `<html>` (persisted to `localStorage`, defaults to `prefers-color-scheme`)
- `src/assets/` — project screenshots, optimized at build time via `astro:assets` `<Image>`
- `public/` — static assets served as-is at the site root, including `_headers` (immutable caching for the content-hashed `/_astro/*` output)
- TypeScript uses Astro's `strict` preset (`tsconfig.json` extends `astro/tsconfigs/strict`)

## Deployment

Cloudflare Workers static assets, configured by `wrangler.jsonc`. There is **no Worker script**: the
config declares only `assets.directory: "./dist"`, so Cloudflare serves the built files directly.

Keep it that way. Adding the `@astrojs/cloudflare` adapter — as Cloudflare's "workers-autoconfig" PR
once did — breaks the project screenshots: the adapter defaults to `imageService: 'cloudflare-binding'`,
which stops `astro:assets` from optimizing images at build time and instead emits runtime
`/_image?href=...` URLs. Nothing serves that route in a static build, so every screenshot 404s.

If an adapter ever becomes genuinely necessary (it is not, while the site stays fully prerendered),
pass `cloudflare({ imageService: 'compile' })` to keep build-time optimization.

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)
