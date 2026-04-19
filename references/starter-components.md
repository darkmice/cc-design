# Starter Components

Use `copy_starter_component` to drop ready-made scaffolds into the project instead of hand-drawing device bezels, deck shells, or presentation grids. The tool echoes the full content back so you can immediately slot your design into it.

Kinds include the file extension — some are plain JS (load with `<script src>`), some are JSX (load with `<script type="text/babel" src>`). Pass the extension exactly.

| Component | Extension | Type | When to Use |
|---|---|---|---|
| `deck_stage` | `.js` | Plain JS | ANY slide presentation — handles scaling, keyboard nav, slide-count overlay, speaker-notes postMessage, localStorage persistence, print-to-PDF |
| `design_canvas` | `.jsx` | JSX (React) | Presenting 2+ static options side-by-side — a grid layout with labeled cells for variations |
| `ios_frame` | `.jsx` | JSX (React) | Design needs to look like a real iPhone screen — includes device bezel with status bar and keyboard |
| `android_frame` | `.jsx` | JSX (React) | Design needs to look like a real Android phone screen — includes device bezel with status bar and keyboard |
| `macos_window` | `.jsx` | JSX (React) | Desktop window chrome with traffic lights |
| `browser_window` | `.jsx` | JSX (React) | Desktop window chrome with tab bar |
| `animations` | `.jsx` | JSX (React) | Timeline-based animation engine (Stage + Sprite + scrubber + Easing) — for animated video or motion-design output |

## Animations Detail

When using `animations.jsx`, you get: `<Stage>` (auto-scale + scrubber + play/pause), `<Sprite start end>`, `useTime()`/`useSprite()` hooks, `Easing`, `interpolate()`, and entry/exit primitives. Build scenes by composing Sprites inside a Stage.

Only fall back to Popmotion (`https://unpkg.com/popmotion@11.0.5/dist/popmotion.min.js`) if the starter genuinely can't cover the use case.

## Deck Stage Detail

For slide decks, do not hand-roll scaling — call `copy_starter_component` with `kind: "deck_stage.js"` and put each slide as a direct child `<section>` of the `<deck-stage>` element. The component handles:
- Scaling (fixed 1920x1080 canvas letterboxed on black via `transform: scale()`)
- Keyboard/tap navigation
- Slide-count overlay
- localStorage persistence
- Print-to-PDF (one page per slide)
- Auto-tags every slide with `data-screen-label` and `data-om-validate`
- Posts `{slideIndexChanged: N}` to parent for speaker notes sync

Prev/next controls must be **outside** the scaled element so they stay usable on small screens.

## Speaker Notes

When adding speaker notes for slides (only when explicitly asked), add to `<head>`:

```html
<script type="application/json" id="speaker-notes">
[
    "Slide 0 notes",
    "Slide 1 notes"
]
</script>
```

The page MUST call `window.postMessage({slideIndexChanged: N})` on init and on every slide change. `deck_stage.js` handles this automatically.
