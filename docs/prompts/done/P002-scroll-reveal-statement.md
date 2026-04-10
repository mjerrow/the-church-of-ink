# P002 — Scroll Reveal: Brand Statement + Signature Animation
# Status: READY
# Owner: MJ
# Touches: index.html (modify existing)

---

## Context

The landing page (P001) is live with a single full-viewport coming-soon section.
It needs a second section revealed on scroll — a personal statement from Madi in
her own voice, followed by an animated signature drawn from her actual initials.

The first viewport remains untouched. The second section slides into view when
the visitor scrolls down.

Read `AGENTS.md` and `docs/BRAND-FOUNDATION.md` for full brand context.

---

## Task

Modify the existing `index.html` to add:
1. A scroll indicator on the first viewport
2. A second full-viewport section with Madi's brand statement
3. An animated SVG signature that draws itself on screen

---

## Changes to Existing Page

### Remove `overflow: hidden` from body

The current body style has `overflow: hidden`. Change to `overflow-x: hidden`
(prevent horizontal scroll but allow vertical). Keep `min-height: 100vh`.
The first section should still fill the viewport exactly.

### Wrap the first section

The current `.content` main element needs to become the first section of a
two-section page. Wrap or restructure so:

- Section 1 (hero): the existing content, `min-height: 100vh`, unchanged visually
- Section 2 (statement): the new content, `min-height: 100vh`

### Fixed background behavior (critical)

The `.ink-water` background, `.hand-watermark`, and body `::before` texture are
all `position: fixed`. They MUST remain fixed. This means:

- The ghost hand watermark stays anchored in the center of the viewport at all
  times — it does NOT scroll with the content
- As the user scrolls from section 1 to section 2, the hero text scrolls away
  and the statement text scrolls into view, but the hand remains static behind
  both — it is the constant, the ever-present mark
- The ink-water purple glow and background texture also remain fixed
- Section 2 has a transparent background so the fixed elements show through
- Do NOT move, hide, or fade the hand watermark during scroll

### Scroll indicator

After the scripture reference at the bottom of section 1, add a subtle scroll
indicator. This should:

- Appear LAST in the reveal sequence (after Isaiah 49:16 finishes animating)
- Be a thin vertical line, approximately 24px tall, centered horizontally
- `position: absolute` within section 1 (not fixed), `bottom: 2rem`, centered
- Color: #e8e4e0 at 15% opacity
- Has a gentle infinite animation: slowly extends and fades, then resets — like
  a drip of ink falling. Use a translateY and opacity animation over ~2.5s.
- Disappears when the user begins scrolling (use a small JS scroll listener
  that adds a class to hide it)

---

## Section 2: The Brand Statement

### Layout

Full viewport height. Same dark background (transparent — the fixed background
elements show through). Content vertically centered. Horizontally centered with
a max-width of approximately 580px. Padding: 2rem on sides.

### Content

Two paragraphs of text, followed by the animated signature.

**Paragraph 1:**

> I believe that art done with heart and integrity is an offering — to the
> person wearing it and to God who gave the talent. Tattoo artistry isn't
> rebellion. It's a craft built on skill, discipline, and purpose. And when
> it's done right, it's an act of service.

**Paragraph 2:**

> The Church of Ink is where craft becomes calling, and the work itself
> becomes worship.

### Typography

- Font: Cormorant Garamond (already loaded), italic, weight 300
- Size: `clamp(0.95rem, 2.2vw, 1.15rem)` — readable but not large
- Line height: 1.8 — generous, airy, gives the words room to breathe
- Color: #e8e4e0 at 50% opacity — softer than the wordmark, these are
  personal words, not a title
- Paragraph spacing: 1.5rem between the two paragraphs
- Text-align: center

### Scroll-triggered reveal (inscribe effect)

The text should NOT be visible when the page loads. It reveals as the user
scrolls section 2 into view using the SAME inscribe animation from section 1 —
the clip-path left-to-right wipe that makes text look like it's being written
into existence. The whole page should feel like one continuous act of
inscription. The pen never stops.

Use the Intersection Observer API to trigger:

- When section 2 enters the viewport (threshold: 0.2), trigger the reveal
- **Paragraph 1:** Inscribes left-to-right using the same `clip-path: polygon`
  animation used for the wordmark in section 1. Duration: 1.5s (slightly
  slower than the wordmark since there's more text). Easing:
  `cubic-bezier(0.2, 0.0, 0.3, 1)` — same as the wordmark.
- **Paragraph 2:** Inscribes with the same clip-path wipe, starting 0.5s after
  paragraph 1 completes.
- **Signature:** Draw animation begins 0.6s after paragraph 2 completes.

Do NOT use fade-in or translateY drift for the text. Use the inscribe
clip-path wipe. Consistency with section 1 is the priority — the entire page
should feel like one continuous inscription from top to bottom.

---

## The Signature Animation

After the two paragraphs, render Madi's signature as an animated SVG that
draws itself on screen.

### What the signature looks like

From Madi's signed artwork (the "Black Death" plague doctor drawing, 2023),
her signature is a flowing "mj" ligature — lowercase cursive "m" that flows
directly into the vertical stroke of a "j". The j's descender becomes a tall
upward slash that crosses through the letters. It reads as one continuous pen
movement — the pen never lifts. After the slash there is often a date, but
for the brand we use only the "mj" mark without a date.

The close-up of her signature shows:
- A lowercase cursive "m" with two rounded humps
- The final stroke of the "m" flows directly into a "j"
- The "j" has a long descending tail that curves back under
- A tall diagonal slash crosses through the entire mark from lower-left
  to upper-right
- The whole thing is one fluid, confident stroke

### SVG implementation

Create an inline SVG path that replicates the feel of her "mj" signature.
The path should be a SINGLE continuous `<path>` element — one unbroken stroke,
because that's how she draws it. It should feel organic and hand-drawn — not
a font, not geometric. Slight irregularity and natural curvature.

Approximate construction:
1. Start at the left, curve up into the first hump of the "m"
2. Drop down and curve into the second hump of the "m"
3. The final downstroke of the "m" transitions into the "j" stem
4. The "j" descends and curves back to the left underneath
5. A long diagonal slash crosses through from lower-left to upper-right

- Total rendered width: approximately 80-100px
- Color: #e8e4e0 at 35% opacity
- Stroke width: 1.5px
- Fill: none
- Stroke-linecap: round (pen-like endpoints)

### Draw animation

Use the SVG stroke-dasharray / stroke-dashoffset technique:

1. Set `stroke-dasharray` to the total path length
2. Set `stroke-dashoffset` to the total path length (fully hidden)
3. Animate `stroke-dashoffset` to 0 over approximately 1.5s
4. Easing: cubic-bezier(0.25, 0.1, 0.25, 1) — smooth, like a pen moving
5. The drawing should feel like watching someone sign their name — starts
   deliberate, accelerates slightly through the slash

### Positioning

- Centered horizontally
- Margin-top: 2.5rem below paragraph 2
- The signature is the last element — nothing follows it

---

## Responsive Behavior

- Mobile (< 480px): Section 2 text hits clamp minimums. Still centered.
  Signature scales down proportionally. Padding: 2rem.
- Tablet / Desktop: Text scales up. Max-width 580px keeps line lengths
  comfortable.
- Both sections should be full viewport height minimum. If section 2 content
  is taller than viewport on very small screens, allow natural scroll — don't
  force overflow hidden.

---

## What NOT To Do

- Do not change any styling or animation in section 1 (the hero)
- Do not move, hide, or fade the hand watermark during scroll — it stays fixed
- Do not add navigation or a back-to-top button
- Do not add any content beyond what is specified
- Do not use a scroll animation library (GSAP, AOS, etc.) — vanilla
  Intersection Observer + CSS animations only
- Do not use fade-in or translateY drift for the statement text — use the
  inscribe clip-path wipe to match section 1
- Do not make the signature animation fast or mechanical — it should feel
  like a real pen signing
- Do not use a font for the signature — it must be an SVG path that looks
  hand-drawn
- Do not include a date after the initials — just the "mj" mark

---

## Success Criteria

- [ ] Page scrolls from section 1 to section 2
- [ ] Section 1 (hero) is visually unchanged
- [ ] Hand watermark stays fixed in viewport center during scroll — does NOT
  move with the content
- [ ] Scroll indicator appears at bottom of section 1 after reveal sequence
- [ ] Scroll indicator disappears when user begins scrolling
- [ ] Section 2 text is hidden until scrolled into view
- [ ] Text reveals with inscribe clip-path wipe (same as section 1 wordmark),
  NOT with fade-in
- [ ] Paragraph 2 inscribes after paragraph 1 completes
- [ ] SVG signature draws itself after text reveals
- [ ] Signature reads as "mj" in a flowing hand-drawn style
- [ ] Signature is a single continuous SVG path (one stroke)
- [ ] The diagonal slash crosses through the letterforms
- [ ] Fixed background elements (ink-water, hand watermark, texture) remain
  visible and stationary across both sections
- [ ] Mobile (375px): both sections fit, text readable, signature visible
- [ ] No external JS libraries added
- [ ] Page still has no horizontal scroll (`overflow-x: hidden` on body)
- [ ] Section 1 existing JS (word-by-word tagline inscription) still works
- [ ] Scroll indicator uses `position: absolute` within section 1, not fixed

---

## Notes

### Why inscribe effect instead of fade-in

The section 1 wordmark uses a clip-path polygon animation that wipes left to
right, making the text look like it's being inscribed. Section 2 should use
the same technique for visual continuity. The entire page should feel like one
continuous act of inscription — the pen never stops. A fade-in with drift
would break that consistency and make section 2 feel like a different page.

### Why the hand watermark stays fixed

The ghost hand is `position: fixed` in the current CSS. During scroll, the hero
text scrolls away and the statement text scrolls in, but the hand stays anchored
in the center of the viewport behind both. It becomes the constant — the
ever-present mark. This is intentional: the hand (and eventually Madi's logo)
is always there, behind everything, like a watermark on sacred paper.

### Why Intersection Observer over scroll events

Intersection Observer is more performant than scroll event listeners for
triggering animations. It fires once when the element enters the viewport
rather than on every scroll tick. It's supported in all modern browsers.

### The signature as brand asset

The SVG "mj" signature is based on the style visible in Madi's signed artwork
(plague doctor "Black Death", 2023). The path is an approximation — once Madi
sees this and likes the draw-on animation, she can sign her actual signature
on paper, it gets traced/vectorized to an SVG path, and swapped in. The
animation mechanism stays the same — only the path data changes. Her real
signature will have the natural imperfection and weight variation that makes
it unmistakably hers.

### Why "planting that flag" was removed

The original paragraph 2 read: "The Church of Ink is where I'm planting that
flag — a space where craft becomes calling, and the work itself becomes
worship." The "planting that flag" metaphor was removed because it introduces
military/conquest imagery that doesn't align with a brand about art, worship,
and service. The sentence is stronger as a direct declaration.

### Why these specific paragraphs

These two paragraphs were selected from a larger set of brand language
developed in the brand foundation session. They are written in first person
(Madi's voice) and carry the creed, the professional standard, and the brand
declaration in conversational language. They were chosen by MJ over other
options that included biographical details — the decision was to keep the
scroll reveal as a statement of purpose, not a bio.
