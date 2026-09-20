# Tailwind Next.js Starter Blog

A modern, highly customizable personal website and blogging platform built with Next.js (App Router), Tailwind CSS, Contentlayer2, and Pliny.

## Overview

This repository powers **Gene Zhang's personal blog** (`https://blog.dailybetter.cn/`), customized from the open-source `tailwind-nextjs-starter-blog` (v2.x). It features full MDX support with syntax highlighting, math formatting (KaTeX), citation support, search via KBar, Giscus comments, and RSS feed generation.

### Key Technologies

- **Framework:** Next.js (App Router, React Server Components)
- **UI & Styling:** Tailwind CSS, `@tailwindcss/typography`, `@tailwindcss/forms`
- **Content Management:** Contentlayer2 (`contentlayer2`, `next-contentlayer2`)
- **Blog Engine & Utilities:** Pliny (`pliny`)
- **Markdown / MDX:** Remark (GFM, Math, Github Alerts) & Rehype (Prism Plus, KaTeX, Citation, Slug, Autolink Headings)
- **Search:** KBar (in-memory search indexing from generated `search.json`)
- **Language & Runtime:** TypeScript, Node.js, Yarn Berry (v3.6.1)

---

## Project Structure

```
.
├── app/                  # Next.js App Router routes and pages
│   ├── layout.tsx        # Root layout (themes, fonts, metadata, header/footer)
│   ├── page.tsx          # Homepage entry
│   ├── Main.tsx          # Homepage post listing layout
│   ├── blog/             # Blog routes (post listing, pagination, [...slug] reader)
│   ├── tags/             # Tag listing and per-tag post views
│   ├── about/            # About page loading author data
│   ├── projects/         # Showcase projects page
│   ├── tag-data.json     # Auto-generated tag count cache (from contentlayer)
│   └── sitemap.ts        # Dynamic sitemap generation
├── components/           # Reusable UI components
│   ├── MDXComponents.tsx # Custom components mapped into MDX renderers
│   ├── Header.tsx        # Navigation header & mobile menu
│   ├── Footer.tsx        # Footer with social links
│   ├── Tag.tsx           # Tag chips with links
│   ├── ThemeSwitch.tsx   # Light/dark mode toggle
│   └── SearchButton.tsx  # Search modal trigger
├── contentlayer.config.ts# Contentlayer schemas, plugins, and build hooks
├── css/                  # Global styles (tailwind.css, prism.css)
├── data/                 # Content sources and site configuration
│   ├── blog/             # Categorized MDX blog posts (e.g. ai, algorithms, golang, etc.)
│   ├── authors/          # Author profile files (e.g. default.mdx)
│   ├── headerNavLinks.ts # Header navigation link definitions
│   ├── projectsData.ts   # Project listing data
│   └── siteMetadata.js   # Site-wide settings (title, author, comments, analytics, SEO)
├── layouts/              # MDX and post list presentation templates
│   ├── PostLayout.tsx    # Standard blog post layout with author sidebar & navigation
│   ├── PostSimple.tsx    # Simplified single-column blog post layout
│   ├── PostBanner.tsx    # Post layout with large hero banner
│   ├── AuthorLayout.tsx  # About / author biography layout
│   └── ListLayoutWithTags.tsx # Filterable post list layout with tag sidebar
├── public/               # Static assets (images, favicons, search index)
└── scripts/              # Build scripts (postbuild.mjs for RSS generation)
```

---

## Commands & Workflow

### Package Management

This project uses **Yarn 3 (Berry)**. Run all package manager commands using `yarn`:

```bash
# Install dependencies
yarn install
```

### Development

```bash
# Start Next.js development server (runs on port 3002)
yarn dev

# Alternatively via Makefile
make run
```

### Production Build & Post-Build

```bash
# Full build: contentlayer build -> next build -> scripts/postbuild.mjs (generates RSS feed)
yarn build

# Alternatively via Makefile
make build

# Start production server locally
yarn serve
```

### Linting & Formatting

```bash
# Run ESLint across source directories and apply automatic fixes
yarn lint

# Prettier check and formatting
yarn prettier --write .
```

Pre-commit hooks are configured via **Husky** and **lint-staged** to run ESLint and Prettier automatically on staged files.

### Deployment

- Committing and pushing changes to the `main` branch automatically triggers deployment (via Vercel auto-deploy).

---

## Content Management Guidelines

### Writing Blog Posts

All blog posts reside under `data/blog/`, grouped into category directories (e.g., `data/blog/ai/`, `data/blog/golang/`, `data/blog/algorithms/`).

#### Required Frontmatter

```yaml
---
title: 'Title of the Post'
date: 'YYYY-MM-DD'
tags: ['tag1', 'tag2']
draft: false
summary: 'A brief summary for previews, SEO, and search index.'
---
```

#### Optional Frontmatter Fields

- `lastmod`: Date modified (`YYYY-MM-DD`).
- `images`: Array of preview images (used for Twitter card and SEO previews).
- `authors`: Array of author IDs matching `data/authors/<author>.mdx` (defaults to `['default']`).
- `layout`: Layout template to use (`PostLayout`, `PostSimple`, `PostBanner`).
- `canonicalUrl`: Canonical URL if published elsewhere.
- `bibliography`: File name of a `.bib` file in `data/` for academic citations.

### Managing Author Profiles

Author markdown files live in `data/authors/` (e.g., `data/authors/default.mdx`). Frontmatter fields:

- `name` (required)
- `avatar` (path to image in `public/static/images/`)
- `occupation`, `company`, `email`, `twitter`, `linkedin`, `github`

### Standalone HTML Pages & Visual Charts

Independent HTML pages (e.g., visual charts, canvas exports) live in `public/charts/`:

- `public/charts/index.html`: Index page listing available standalone visualizations (accessible at `/charts`).
- `public/charts/style.css`: Shared stylesheet for standalone charts.
- `public/charts/<name>.html`: Direct standalone page (accessible at `/charts/<name>.html`).

### Site Configuration

Global site settings (author, site URL, social accounts, theme default, comments with Giscus, analytics, search) are configured in:
`data/siteMetadata.js`

Header navigation links are defined in:
`data/headerNavLinks.ts`

---

## Development & Code Conventions

1. **Next.js App Router Architecture:**
   - Pages and server components reside in `app/`.
   - Client components must include `'use client'` directive at the top (e.g., interactive widgets, hooks, theme switchers).
   - Layout components in `layouts/` format the MDX content parsed by Contentlayer.
2. **Styling:**
   - Use Tailwind CSS utility classes.
   - Global typography rules and prose customizations are located in `tailwind.config.js`.
   - Dark mode uses class-based switching (`darkMode: 'class'`).
3. **Contentlayer Generated Types:**
   - Generated document types and metadata are output to `.contentlayer/generated` and aliased to `contentlayer/generated`.
   - Whenever frontmatter schemas or MDX plugins change, update `contentlayer.config.ts` and test with `yarn build`.
4. **Git & Commit Hygiene:**
   - Do not stage or commit without explicit user instruction.
   - Ensure `yarn lint` passes cleanly before committing changes.
