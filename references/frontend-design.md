# Frontend Design

> **Load when:** Project has no design system, or user says "make it look good" / "design a screen" without specifying visual direction
> **Skip when:** Project has existing DESIGN.md, token files, or CSS variable system
> **Why it matters:** Without a design system, AI defaults to generic web tropes (Inter, blue gradients, rounded containers). This reference gives concrete aesthetic starting points.
> **Typical failure it prevents:** Generic-looking output that violates the "avoid AI slop" guideline; fonts and colors that clash instead of harmonize

## Font Selection

Avoid: Inter, Roboto, Arial, Fraunces, system-ui (as primary identity font).

### By project type

| Type | Primary | Secondary | Rationale |
|------|---------|-----------|-----------|
| Startup / SaaS | `DM Sans` | `Source Serif 4` | Clean geometric, professional serif for emphasis |
| Creative / Portfolio | `Space Grotesk` | `Playfair Display` | Distinctive geometric sans, classic serif contrast |
| Technical / Dev tool | `JetBrains Mono` | `Outfit` | Monospace identity, modern sans for body |
| Presentation / Deck | `Bricolage Grotesque` | `Instrument Serif` | Warm grotesque, elegant serif |
| Mobile app UI | `Plus Jakarta Sans` | `Fraunces` (only as accent) | Friendly sans, quirky serif accent |
| Minimal / Editorial | `Sora` | `Cormorant Garamond` | Precise sans, refined serif |

Load from Google Fonts: `<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&family=Source+Serif+4:wght@400;600&display=swap" rel="stylesheet">`

## Color System (oklch)

When no design system exists, build a palette using oklch for perceptual uniformity.

### Method

1. Pick one **identity color** (the brand/accent) — ask the user or pick from context
2. Generate 3 harmonious colors using oklch hue rotation:
   - Complement: rotate hue by 180°
   - Analogous: rotate by ±30°
3. Set lightness range: surface=0.95, text=0.15, muted=0.6, accent=0.55
4. Map to CSS custom properties

```css
:root {
  --surface: oklch(0.95 0.02 hue);
  --text:    oklch(0.15 0.02 hue);
  --muted:   oklch(0.60 0.04 hue);
  --accent:  oklch(0.55 0.18 hue);
  --accent-light: oklch(0.85 0.08 hue);
  --border:  oklch(0.80 0.02 hue);
}
```

Replace `hue` with the actual value (0-360). Example: warm orange = hue 55.

### Common starting palettes

| Mood | Hue | Accent oklch | Use case |
|------|-----|-------------|----------|
| Warm | 55 | oklch(0.55 0.18 55) | Consumer, lifestyle |
| Cool | 240 | oklch(0.55 0.18 240) | Enterprise, data |
| Fresh | 150 | oklch(0.55 0.15 150) | Health, sustainability |
| Bold | 25 | oklch(0.60 0.22 25) | Entertainment, gaming |
| Neutral | 0 (achromatic) | oklch(0.50 0.02 0) | Dev tools, docs |

## Spacing System

Use an 8px base grid. Scale by multiples of 4px for density control.

| Token | Value | Use |
|-------|-------|-----|
| `--space-1` | 4px | Icon gaps, tight inline |
| `--space-2` | 8px | Button padding, list items |
| `--space-3` | 16px | Card padding, section gaps |
| `--space-4` | 24px | Section padding, card margins |
| `--space-5` | 32px | Page margins, hero spacing |
| `--space-6` | 48px | Section breaks |
| `--space-7` | 64px | Full-bleed separators |

## Composition Rules

1. **Contrast hierarchy**: Headings 2-3x body size. Body 16-18px for web, 24px+ for 1920x1080 slides.
2. **Visual weight**: One element dominant per section. Everything else supports it.
3. **Whitespace ratio**: 60% space, 40% content for hero/cover. 40% space, 60% content for data-dense sections.
4. **Alignment**: One alignment per section (left, center, or right). Mix across sections, not within.
5. **Depth**: Use only 1 shadow level across the entire page. Either `0 1px 3px rgba(0,0,0,0.1)` for subtle or `0 8px 30px rgba(0,0,0,0.12)` for dramatic. Never both.
6. **Borders**: Prefer `1px solid var(--border)` over shadows for separation in dense layouts. Shadows for isolation in sparse layouts.