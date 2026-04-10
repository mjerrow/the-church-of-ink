# The Church of Ink — Agent Context

A coming-soon landing page for thechurchofink.com. Tattoo artistry brand rooted
in Christian faith. Belongs to Madi Jerrow (17, artist, ski instructor, strong faith).

## The Brand in 30 Seconds

Madi draws on skin — detailed fine-line ink art with stipple texture and splatter.
She believes art done with integrity is worship, not rebellion. The brand name
makes that claim: the space where ink meets skin is holy ground. The church judging
the artist is the problem she's solving. Isaiah 49:16 is the theological anchor —
God inscribed his people on his own palms.

For the full brand story — who Madi is, how every decision was made, and the
theological reasoning behind each element — read `docs/BRAND-FOUNDATION.md`.

## Brand Language

| Element | Text |
|---|---|
| **Tagline** | Sacred work, inscribed on skin. |
| **Creed** | Art done with heart and integrity is an offering — to the person wearing it and to God who gave the talent. |
| **Ethos** | Do the work. Let your character speak louder than perception. |
| **Origin quote** | "Skin feels different when you draw on it." |

**Scripture anchors:**
- Isaiah 49:16 — "I have inscribed you on the palms of my hands" (tagline)
- 1 Timothy 4:12 — don't dismiss the young (identity)
- Colossians 3:23 — work as unto the Lord (vocation)
- 1 Corinthians 13:13 — the greatest of these is love (mission)

## Brand Aesthetic

Dark, moody, organic. Gothic-botanical. Imperfect on purpose.
NOT clean vector. NOT corporate. NOT churchy.

**Colors:**
- Background: #0a0a0a (true black)
- Text: #e8e4e0 (warm cream)
- Accent: deep purple, #7B5EA7–#9B7EC8 range (Madi's hair — used sparingly)
- Tertiary: #6b6560 (muted warm gray)

**Typography:** Distinctive serif (Cormorant Garamond or similar). Google Fonts.
Never Inter, Roboto, Arial, or generic sans-serif as primary.

**Logo:** In development. Madi will hand-draw the final mark. Wordmark only for now.

## Architecture

**Stack:** Static HTML/CSS/JS. Single `index.html`. No framework, no build step.

**Hosting:** Cloudflare Pages (free tier). DNS already on Cloudflare.

**Domain:** thechurchofink.com

**Deploy:** Push to `main` → Cloudflare Pages auto-deploys.

## Repo Structure

```
the-church-of-ink/
├── index.html                              # the page — all HTML/CSS/JS inline
├── AGENTS.md                               # this file (operating manual)
├── CLAUDE.md                               # commit conventions, prompt execution rules
├── README.md                               # deploy instructions
└── docs/
    ├── BRAND-FOUNDATION.md                 # full brand story, decisions, theology
    └── prompts/
        ├── README.md                       # prompt index
        ├── drafts/                         # work in progress
        ├── ready/                          # reviewed, ready to execute
        └── done/                           # executed and verified
```

## Design Rules (Absolute)

1. Mobile-first. Full viewport height. Vertically centered.
2. The page "loads" with a deliberate reveal sequence — it does not just appear.
3. No images on v1. Typography and motion carry the page.
4. No nav, no links, no form, no social icons, no portfolio.
5. Purple accent emerges from the dark — atmospheric, not decorative.
6. Animations are slow, smooth, organic. Never bouncy or mechanical.
7. Isaiah 49:16 reference at bottom — just the reference, not the verse.

## What Is NOT On This Page

No logo image (yet), no portfolio, no contact form, no email capture,
no social links, no about section, no navigation.
