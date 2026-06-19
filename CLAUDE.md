# CLAUDE.md

This file gives Claude Code context for working in this repository. Claude Code reads this automatically when opened in this project.

## Project Overview

Business website for **Kyle Goetz Land Surveys**, a sole-proprietor land surveying practice.

- **Surveyor:** Kyle Goetz, PLS 10078 (California)
- **Service area:** Primarily Santa Cruz County, CA; some work in Monterey County
- **Services:** Boundary surveys, SB9 lot splits, corner records, Records of Survey, deed/title analysis
- **Brand tone:** Professional, precise, understated — no marketing hype. Copy should match Kyle's voice: concise and direct.

## Tech Stack

- **Static site generator:** Hugo v0.153.2-extended
- **Theme:** `hugo-universal-theme` (third-party, installed as a git submodule at `themes/hugo-universal-theme`)
- **CSS framework:** Bootstrap grid (via theme); custom overrides in `static/css/custom.css`
- **Contact form:** Formspree (`https://formspree.io/f/mdaogowd`), no custom backend
- **Hosting/CI-CD:** AWS Amplify, Git-based — pushing to the connected branch triggers a build and deploy automatically

## Common Commands

```bash
# Local dev server with live reload
hugo server -D

# Production build (outputs to public/)
hugo --minify

# Check Hugo version in use
hugo version
```

Amplify runs `hugo --minify`, outputs to `public/`, no base directory override. This matches the local build command exactly.

## Repository Structure

- `content/` — Markdown pages
  - `about.md`, `contact.md`, `faq.md`
  - `services/boundary/index.md`
  - `services/topographic-mapping/index.md`
  - `services/construction-staking/index.md`
- `layouts/` — Custom Hugo template overrides (take precedence over theme)
  - `index.html` — homepage
  - `_default/single.html`, `_default/list.html`
  - `partials/` — breadcrumbs, carousel, contact, cta, footer, headers, nav, schema-local, top
  - `shortcodes/faq.html`
  - `robots.txt`, `sitemap.xml` — custom implementations
- `data/carousel/` — YAML files driving the homepage carousel (one per service)
- `data/features/` — YAML files driving the homepage features block (one per service)
- `static/images/` — All site images; `_archive/` subfolder holds unused originals
- `static/css/custom.css` — Site-specific style overrides
- `hugo.toml` — Site configuration (menus, params, Formspree endpoint, Google Maps key)
- `amplify.yml` — Amplify build spec
- No `assets/` directory — not using Hugo Pipes

## Conventions

- Match existing Hugo front matter schema and shortcode usage — don't introduce a new pattern without flagging it first.
- Keep Bootstrap class usage consistent with the rest of the site rather than introducing a parallel styling approach.
- Homepage content is driven by `data/carousel/*.yaml` and `data/features/*.yaml`, not by the content Markdown files.
- Don't add new dependencies (JS libraries, Hugo modules, npm packages) without flagging the tradeoff first. This site should stay lightweight and static.
- Never hardcode secrets, API keys, or the Formspree endpoint in a way that diverges from how it's currently handled.
- Content edits should be factual and concise — no marketing fluff.
- `disableKinds = ["taxonomy", "term"]` is set — tags and categories are disabled; don't add them.

## Guardrails

- **Ask before changing:** `amplify.yml`, build/deploy configuration, DNS/domain settings, or anything that could break the production build.
- **Proceed directly on:** content-only edits (copy changes, new text blocks, image swaps).
- **Always confirm:** the license number (PLS 10078), service area, and contact details are accurate before publishing changes that touch them.
- Default to minimal, targeted diffs over wholesale rewrites unless a redesign is explicitly requested.

## SEO Priorities

- Local search intent: "land surveyor Santa Cruz County," "land surveyor Monterey County," SB9 lot split, boundary survey, corner record, Records of Survey.
- LocalBusiness structured data via `layouts/partials/schema-local.html`.
- Meta titles/descriptions per page, descriptive alt text, accurate sitemap.xml/robots.txt.
