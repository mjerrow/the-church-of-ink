# P001 — Landing Page: Coming Soon with Reveal Sequence
# Status: READY
# Owner: MJ
# Touches: index.html (new file)

---

## Context

The Church of Ink needs a coming-soon landing page on thechurchofink.com. This is
a brand statement, not an interface — typography and motion carry the entire page.
No images, no form, no nav. The page should feel like something being drawn into
existence, not a website rendering.

MJ's creative brief for this page: *"I want the page to have identity, motion,
load style — I want the page to 'load', not appear."* He was deliberately vague
on technique because the feeling mattered more than the method. The reveal
sequence is the answer to that brief.

Read `AGENTS.md` first. Every decision in this prompt is grounded in that file.

---

## Task

Create a single `index.html` file at the project root. All CSS and JS inline.
No external dependencies except Google Fonts (loaded via `<link>` in `<head>`).

The file must be valid HTML5: `<!DOCTYPE html>`, `<html lang="en">`,
`<meta charset="UTF-8">`.

---

## The Page

Full viewport height (`min-height: 100vh`, NOT `height: 100vh` — allows graceful
overflow on very small screens). Vertically and horizontally centered content stack
using flexbox (`display: flex; flex-direction: column; align-items: center;
justify-content: center;`).

True black background (#0a0a0a). Mobile-first. No horizontal or vertical scrollbar
on any viewport 375px or wider.

### Content stack (top to bottom, centered):

1. **Wordmark:** "THE CHURCH OF INK" — Cormorant Garamond (Google Fonts),
   uppercase, font-weight 300, letter-spacing 0.35em.
   Size: `clamp(1.4rem, 5vw, 2.2rem)`. Color: #e8e4e0.
   `text-align: center; line-height: 1.2;` — the wordmark may wrap on narrow
   screens and must remain centered and readable if it does.

2. **Thin divider line:** Use a `<div>` (not `<hr>`). Width: 60px. Height: 0.5px.
   Background-color: `rgba(232, 228, 224, 0.25)`. Centered via `margin: 1.5rem auto`.

3. **Tagline:** "Sacred work, inscribed on skin." — Cormorant Garamond,
   italic, weight 300, letter-spacing 0.15em.
   Size: `clamp(0.75rem, 2.5vw, 1rem)`. Color: `rgba(232, 228, 224, 0.55)`.
   Include the period.

4. **Spacer:** `margin-top: 4rem` on the next element (not an empty div).

5. **Coming Soon:** "COMING SOON" — Cormorant Garamond, uppercase, weight 300,
   letter-spacing 0.3em. Size: `clamp(0.55rem, 1.5vw, 0.7rem)`.
   Color: `rgba(232, 228, 224, 0.20)`.

6. **Scripture reference:** "Isaiah 49:16" — Cormorant Garamond, italic,
   weight 300, letter-spacing 0.08em. Size: `clamp(0.5rem, 1.2vw, 0.6rem)`.
   Color: `rgba(232, 228, 224, 0.15)`. Margin-top: 0.75rem.
   Just the reference text — do NOT include the verse itself.

### Background texture

Not flat black. Layer a subtle CSS radial-gradient noise pattern over #0a0a0a.
Implementation: use a `::before` pseudo-element on `body` with a repeating
radial-gradient that creates a fine dot/grain pattern. Example approach:

```css
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background: radial-gradient(circle, rgba(232,228,224,0.015) 1px, transparent 1px);
  background-size: 3px 3px;
  pointer-events: none;
  z-index: 0;
}
```

The texture must be barely perceptible — visible only on close inspection, not
as an obvious pattern. Adjust opacity if the dots are too visible. No image files.
The content stack must sit above this layer (`position: relative; z-index: 1`).

### Purple accent glow

A single centered radial gradient behind the content stack. Use a `::after`
pseudo-element on the content wrapper, or a dedicated absolutely-positioned div.

```css
background: radial-gradient(
  ellipse 50vw 50vh at center,
  rgba(123, 94, 167, 0.08),
  transparent 70%
);
```

The glow must feel like purple light bleeding through darkness from behind — not
painted on top, not a spotlight, not a visible circle or disc. There must be no
perceptible edge where the glow ends. It bleeds into the surrounding black.
Do not exceed `rgba(123, 94, 167, 0.10)` at peak opacity.
Position: centered on the content stack. Size: at least 50vw wide, 50vh tall.
`pointer-events: none; z-index: 0`.

---

## The Load Sequence

The page reveals itself in a deliberate sequence. Every element starts invisible
(`opacity: 0`) and animates in via CSS `@keyframes` with `animation-delay`.
Use `animation-fill-mode: forwards` so elements stay visible after animating in.

### Timing:

| Window       | Element          | Effect |
|-------------|-----------------|--------|
| 0.0s–0.8s   | Nothing          | Pure black. The page breathes before anything appears. |
| 0.8s–1.2s   | Background + glow | Texture and purple glow fade in. `opacity: 0 → 1`. Duration: 0.4s. Delay: 0.8s. |
| 1.5s–2.5s   | Wordmark         | Clip-path wipe left-to-right. Duration: 1.0s. Delay: 1.5s. See details below. |
| 2.8s–3.2s   | Divider line     | Draws from center outward. `scaleX(0) → scaleX(1)`. Duration: 0.4s. Delay: 2.8s. |
| 3.4s–4.0s   | Tagline          | Fade in + drift up: `opacity: 0 → 1` and `translateY(8px) → translateY(0)`. Duration: 0.6s. Delay: 3.4s. |
| 5.0s–5.5s   | Coming Soon      | Same as tagline but subtler: `translateY(4px) → translateY(0)`. Duration: 0.5s. Delay: 5.0s. |
| 5.5s–6.0s   | Scripture ref    | Opacity only: `opacity: 0 → 1`. Duration: 0.5s. Delay: 5.5s. |

### Wordmark clip-path animation (the hero moment):

```css
@keyframes inscribe {
  from { clip-path: polygon(0 0, 0 0, 0 100%, 0 100%); }
  to   { clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%); }
}

.wordmark {
  animation: inscribe 1s cubic-bezier(0.25, 0.1, 0.25, 1) 1.5s forwards;
  clip-path: polygon(0 0, 0 0, 0 100%, 0 100%); /* start hidden */
}
```

The second point in the `from` polygon is `0 0` (not `0% 0`) — this makes the
text fully clipped. Do NOT use `opacity` for the wordmark reveal; the clip-path
IS the reveal. The wordmark should NOT also fade in — it wipes in only.

### Divider line animation:

```css
.divider {
  transform: scaleX(0);
  animation: draw-line 0.4s cubic-bezier(0.25, 0.1, 0.25, 1) 2.8s forwards;
}
@keyframes draw-line {
  to { transform: scaleX(1); }
}
```

### Post-reveal continuous animation:

After full reveal (after ~6s), the purple glow opacity oscillates:

```css
@keyframes glow-pulse {
  0%, 100% { opacity: 0.6; }  /* relative to the glow element's base opacity */
  50%      { opacity: 1; }
}
```

Apply to the glow element with `animation: glow-pulse 8s ease-in-out infinite`.
Delay this animation by 6s so it starts after the reveal sequence completes.
The visible effect should be the glow shifting between roughly 0.06 and 0.10
alpha — barely noticeable, keeps the page alive.

### Easing:

Use `cubic-bezier(0.25, 0.1, 0.25, 1)` for all reveal animations.
Nothing bouncy, springy, or snappy. Smooth and deliberate.

### Reduced motion:

Respect `prefers-reduced-motion: reduce`. When active, skip all animations and
show the page fully revealed immediately (all elements visible, no delays).

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation: none !important;
    transition: none !important;
    opacity: 1 !important;
    transform: none !important;
    clip-path: none !important;
  }
}
```

---

## Responsive Behavior

- **Mobile (< 480px):** `padding: 0 2rem` on the content wrapper. Clamp minimums
  apply. Load sequence plays identically to desktop.
- **Tablet (480px–768px):** Text scales up naturally via clamp. No layout changes.
- **Desktop (> 768px):** Clamp maximums cap text sizes. Content remains centered
  with ample breathing room.
- The page must not scroll on any viewport 375px wide and taller than 500px.
  Use `overflow: hidden` on `body` — but if content overflows on extremely small
  screens (< 375px), `min-height: 100vh` (not `height: 100vh`) lets it extend
  gracefully rather than clipping.

---

## Meta / Head

Exact `<head>` contents (in this order):

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Church of Ink</title>
<meta name="description" content="Sacred work, inscribed on skin. Coming soon.">
<meta name="robots" content="noindex, nofollow">
<meta property="og:title" content="The Church of Ink">
<meta property="og:description" content="Sacred work, inscribed on skin. Coming soon.">
<meta property="og:type" content="website">
<meta name="theme-color" content="#0a0a0a">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;1,300&display=swap" rel="stylesheet">
```

Notes:
- `font-display=swap` is embedded in the Google Fonts URL via `&display=swap`.
- Only load weights actually used: 300 normal and 300 italic.
- No favicon link (none exists yet).
- `noindex, nofollow` — do not index a coming-soon page.

### Font fallback

Declare the font stack as: `'Cormorant Garamond', 'Georgia', 'Times New Roman', serif`.
If Google Fonts fails to load, the page must still render with a serif fallback.

---

## What NOT To Do

- No JavaScript frameworks or libraries
- No external CSS files — everything inline in a single `<style>` block
- No images or SVGs
- No scroll behavior or scroll-based animations
- No hover effects, cursor changes, or interactive elements
- No loading spinner or progress bar
- No "under construction" language or imagery
- Do not use Inter, Roboto, Arial, or any sans-serif as primary font
- Do not make animations bouncy, springy, or playful
- Do not add any content beyond what is specified above
- Do not add comments in the HTML/CSS unless they mark a section boundary
- Do not use `<hr>` for the divider (use a styled `<div>`)
- Do not use JavaScript for animation — CSS `@keyframes` only
- Do not add aria-hidden to visible content elements
- Do not use `height: 100vh` — use `min-height: 100vh`
- Do not let the aesthetic drift toward occult, tarot, palmistry, or gothic-horror
  territory. The brand is dark and moody but rooted in Christian faith — sacred,
  not spooky. If a visual choice could read as witchcraft or the occult, reject it.
- Do not add pentagrams, moons, stars-as-mysticism, eye symbols, or any imagery
  associated with the occult or new-age spirituality
- Do not make the page feel too polished, too pretty, or too delicate. The brand's
  bloom logo concept was rejected for being "too botanical-illustration, too
  garden-magazine." The same standard applies here — this should feel like
  stepping into a dark, organic space, not a luxury spa or editorial magazine.
  Warm cream on near-black with atmospheric purple, not pristine white on slate

---

## Success Criteria

- [ ] `index.html` exists at project root, valid HTML5, opens in browser with zero console errors
- [ ] `<!DOCTYPE html>` and `<html lang="en">` present
- [ ] Page is full viewport height, content vertically and horizontally centered (flexbox)
- [ ] Wordmark reveals left-to-right via clip-path polygon animation — NOT opacity fade
- [ ] Divider line draws from center outward via `scaleX(0) → scaleX(1)`
- [ ] Tagline fades in with upward drift (translateY 8px → 0)
- [ ] Coming Soon and Scripture ref animate in sequence after tagline
- [ ] Reveal sequence timing matches the table above (within ~0.1s tolerance)
- [ ] Purple glow pulses subtly on infinite loop after full reveal
- [ ] Background has subtle grain texture via CSS pseudo-element, not flat #0a0a0a
- [ ] All text is Cormorant Garamond with Georgia/serif fallback
- [ ] Google Fonts loads only weight 300 normal + 300 italic with `display=swap`
- [ ] `prefers-reduced-motion: reduce` skips all animations, shows page immediately
- [ ] Mobile (375px wide): no scroll, text readable, sequence plays correctly
- [ ] Desktop (1440px wide): text sizes capped at clamp max, centered with room
- [ ] No external dependencies except Google Fonts `<link>`
- [ ] `noindex, nofollow` meta tag present
- [ ] OG tags present (title, description, type)
- [ ] `theme-color` meta tag set to #0a0a0a
- [ ] Zero JavaScript (no `<script>` tags)
- [ ] No images, SVGs, or external files referenced
- [ ] Purple glow has no visible edge — bleeds from behind, not painted on top
- [ ] Content stack occupies ≤60% of viewport height at 375×667 (room for future logo)
- [ ] No occult, tarot, or gothic-horror visual reads — dark and sacred, not spooky

---

## Notes

### Why Cormorant Garamond

Distinctive serif with a sacred, editorial quality. Has weight 300 (light) which
gives the airy, elevated feel the brand needs. Free on Google Fonts. Italic is
beautiful for the tagline. Not overused in the tattoo/alternative space.

### Why no JavaScript for animation

CSS @keyframes with animation-delay handles the entire sequence without a
framework dependency. The page loads faster, deploys as a pure static file,
and there's nothing to break. JS is only needed if we add interactivity later.

### Logo placeholder — vertical space budget

The page launches with typography only. When Madi draws the logo mark, it gets
added above the wordmark as a separate prompt (P002).

To ensure P002 integration doesn't break the layout: the content stack (wordmark
through scripture reference) should occupy no more than 60% of viewport height
on a 375×667 screen (iPhone SE). This leaves ~20% above and ~20% below for
breathing room — and the top 20% is where the logo will eventually sit. Do not
add a visible placeholder element, but do verify the content stack fits within
this budget at mobile size. If it doesn't, reduce the 4rem spacer before
"COMING SOON" first.
