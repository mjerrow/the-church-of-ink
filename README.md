# The Church of Ink

Coming-soon landing page for [thechurchofink.com](https://thechurchofink.com).

## Stack

Static HTML/CSS/JS — single `index.html`, no build step.
Hosted on Cloudflare Pages (free tier). Domain DNS on Cloudflare.

## Deploy

### First-time setup

1. Push repo to GitHub (mjerrow org)
2. Cloudflare dashboard → Pages → Create a project → Connect to Git
3. Select this repo
4. Build settings: leave build command empty, output directory `/`
5. Deploy
6. Custom domains → Add `thechurchofink.com`
7. Cloudflare auto-verifies DNS and provisions SSL

### Updates

Push to `main`. Cloudflare Pages auto-deploys.

## Key Files

```
index.html                    — the page (two sections, all inline)
assets/hand-watermark.png     — ghost hand logo concept (background watermark)
AGENTS.md                     — brand context, page structure, architecture
CLAUDE.md                     — commit conventions, prompt execution rules
docs/BRAND-FOUNDATION.md      — full brand story, decisions, theology
docs/prompts/README.md        — prompt library index
```
