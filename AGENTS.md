# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Note: `CLAUDE.md` is a symlink to `AGENTS.md` — edit `AGENTS.md`.

## Project Overview

A personal portfolio site built with Astro 7 (static output, no framework integrations or adapters configured yet — `astro.config.mjs` is an empty `defineConfig({})`). Requires Node >= 22.12.0.

## Commands

```bash
npm run dev       # Start dev server (see background mode below)
npm run build     # Production build to dist/
npm run preview   # Preview the production build locally
npm run astro     # Run the Astro CLI directly (e.g. npm run astro -- check)
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

- `src/pages/` — file-based routing; each `.astro` file becomes a route (currently only `index.astro`)
- `public/` — static assets served as-is at the site root
- TypeScript uses Astro's `strict` preset (`tsconfig.json` extends `astro/tsconfigs/strict`)

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)
