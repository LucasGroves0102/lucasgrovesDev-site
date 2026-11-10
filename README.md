# lucasgroves.dev

Personal site and writing, built with [Astro](https://astro.build) and hosted on Cloudflare Pages.

## Run it locally

```bash
npm install
npm run dev      # http://localhost:4321
npm run build    # static output in dist/
```

## Add a post

Create a Markdown file in `src/pages/writing/`:

```markdown
---
layout: ../../layouts/Post.astro
title: Post title
date: 2026-11-10
summary: One sentence for the writing list.
---

Post body in Markdown.
```

It appears on the home page and the writing page automatically, newest first.

## Deploy

Cloudflare Pages builds every push to `main`: framework preset Astro, build command `npm run build`, output directory `dist`.
