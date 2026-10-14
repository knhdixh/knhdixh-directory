# knhdixh cookbook

The house style for every site on **knhdixh.com** and its subdomains. Clean, Scandinavian, black on white, with a small number of well-made motions. Reference implementation: https://knhdixh.com ([source](https://github.com/knhdixh/knhdixh-directory/blob/main/public/index.html)). `template.html` in this gist is a working starter.

## 1. Principles

1. **Black on white.** No dark mode, no gradients, no shadows, no cards with borders all round. Colour appears in exactly one place: the blue water trail.
2. **One typeface, two weights.** Inter 400 and 500. Hierarchy comes from size and grey, never from bold, italics, colour or ALL CAPS.
3. **Lowercase names.** Product and page names are lowercase (`epita`, `ciggs`). Sentence case everywhere else.
4. **Few words.** Labels are one or two words. No taglines, no marketing copy, no emoji.
5. **Motion is the decoration.** Every animation uses the same easing, runs once, and is skipped for reduced-motion users.
6. **Narrow and left-aligned.** One column, 520–640px wide, lots of empty space above.

## 2. Tokens

```css
:root {
  --fg: #000;
  --muted: #8a8a8a;      /* secondary text, numbers, meta */
  --line: #e6e6e6;       /* hairlines only */
  --bg: #fff;
  --accent: #0d52ff;     /* only for focus rings and the water trail; never for text or fills */
  --danger: #d0021b;     /* error text only */
  --ease: cubic-bezier(.16, 1, .3, 1);   /* the only easing curve */
}
```

| Role | Size | Weight | Colour | Letter-spacing |
|---|---|---|---|---|
| Display (site name) | `clamp(56px, 13vw, 112px)` | 500 | `--fg` | `-0.055em`, line-height `.9` |
| Page title (inside apps) | `clamp(32px, 6vw, 48px)` | 500 | `--fg` | `-0.04em` |
| Body / rows | 15px | 400, names 500 | `--fg` | `-0.005em`, line-height 1.6 |
| Meta, numbers, footer | 13px | 400 | `--muted` | normal, `tabular-nums` |

Font load: `https://fonts.googleapis.com/css2?family=Inter:wght@400;500&display=swap`, fallback `system-ui, -apple-system, sans-serif`. Always `-webkit-font-smoothing: antialiased`.

## 3. Layout

- `main { max-width: 640px; margin: 0 auto; padding: 20vh 24px 80px; }` (`16vh` under 480px). Tool-style apps may go to 760px, never wider.
- Vertical rhythm: 72px between major blocks, 48px under a page title, 16px row padding.
- Sections start with a **meta row**: label on the left, count or status on the right, 13px muted (`Index … 04`).
- Lists are **hairline rows**: a 1px `--line` rule on top of every row and under the last one. Grid `36px 1fr auto` = number · name · detail.
- Footer: domain on the left, year on the right, 13px muted, 72px above.

## 4. Motion recipes

All timings use `--ease`. Trigger by adding a `ready` class to `main` after two `requestAnimationFrame`s.

| Element | Effect | Duration | Stagger / delay |
|---|---|---|---|
| Display heading | each character rises from an `overflow:hidden` mask (`translateY(105%) → 0`) while cycling random glyphs, then settles | 1.2s | 55ms per char, starts 200ms |
| Text cursor | 0.055em × 0.78em black bar, `steps(1)` blink at 1.05s, appears after the heading settles | – | – |
| Row hairlines | `scaleX(0 → 1)`, origin left | 1.4s | 90ms per row, starts 700ms |
| Row contents | rise from mask (`translateY(110%) → 0`) | 1.1s | 90ms per row, starts 850ms |
| Footer | opacity 0 → 1 | 1s | starts 1.6s |
| Row hover | name decodes left→right (380ms); detail turns `--fg`; `↗` slides in from −6px | 0.5s | – |
| Cursor trail | WebGL water, blue, see §5 | – | – |

Rules:
- Only `transform` and `opacity` animate. Nothing bounces, nothing loops except the text cursor.
- Hover effects also fire on `:focus-visible` / `focus`.
- `@media (prefers-reduced-motion: reduce)`: remove transforms and transitions, show everything immediately, hide the fluid canvas.

## 5. Water cursor trail

A small stable-fluids simulation (curl → vorticity → divergence → 20 pressure iterations → gradient subtract → advect velocity → advect dye) in WebGL2, rendered to a fixed, `pointer-events:none` canvas behind the content (`z-index: 0`, content `z-index: 1`). Copy the whole `/* ---------- fluid ---------- */` block from `template.html`; do not rewrite it.

Defaults (the `CFG` object):

```js
SIM: 128, DYE: 768, VEL_DISSIPATION: 1.4, DYE_DISSIPATION: 2.1,
PRESSURE_ITER: 20, CURL: 14, RADIUS: 0.13, FORCE: 5200, INK: 0.24
```

- Colour: shallow `rgb(115,184,255)` → deep `rgb(13,82,255)`, alpha capped at 0.55, with height-field shading and a small white specular highlight, so it reads as water rather than paint.
- Pointer moves inject velocity and dye; scrolling stirs at the last pointer position.
- The loop stops simulating 6 seconds after the last move.
- Bail out silently (remove the canvas) when WebGL2 or `EXT_color_buffer_float` is missing, a shader fails to compile, or reduced motion is on.
- Larger trail: raise `RADIUS` and lower `DYE_DISSIPATION`. Calmer trail: lower `CURL` and `FORCE`.

## 6. App UI (for tools on subdomains)

The directory page has no forms. Tool apps add these, in the same voice:

- **Buttons.** Primary: black fill, white text, 15px/500, `padding: 12px 18px`, `border-radius: 0`, hover `opacity: .8`. Secondary: text-only black with a 1px underline that grows from the left on hover (same as row names). Never rounded pills or colour fills.
- **Inputs.** No box: a single 1px `--line` bottom border that turns `--fg` on focus. 15px text, label above in 13px `--muted`. Placeholder in `--muted`.
- **Tables.** Hairline rows exactly like the list; headers are 13px `--muted`; numbers right-aligned with `tabular-nums`.
- **Numbers/stats.** Big number in the page-title size, label underneath in 13px muted. No coloured badges; state with words ("on track", "2 left").
- **Feedback.** Toast: black pill-free bar bottom-left, white 13px text, slides up from a mask, auto-hides after 3s. Errors are `--danger` text under the field, never red boxes.
- **Focus.** `outline: 1px solid var(--fg); outline-offset: 4px`.
- **Empty states.** One muted sentence. No illustrations.
- **Icons.** Avoid. If unavoidable, text glyphs (`↗ ← → + ×`) in the body font.
- **404.** Display-size `404`, one muted line "Nothing here.", a `Back` link.

## 7. Checklist before shipping

- [ ] Only Inter 400/500; no other weights or fonts
- [ ] Only `--fg`, `--muted`, `--line`, `--bg` visible on the page (plus the trail)
- [ ] Names lowercase, copy under five words per label
- [ ] Heading reveal, hairline draw-in and row rise use `--ease` and the timings above
- [ ] Water trail present on marketing/landing pages; optional inside dense tools
- [ ] Reduced motion shows everything instantly, fluid hidden
- [ ] No horizontal scroll at 390px; no console errors
