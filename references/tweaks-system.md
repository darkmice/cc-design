# Tweaks System

Tweaks let the user toggle in-page controls from the toolbar to adjust design aspects — colors, fonts, spacing, copy, layout variants, etc. You design the Tweaks UI; it lives inside the prototype.

## Protocol

**Order matters: register the listener before you announce availability.**

1. **First**, register a `message` listener on `window`:
   ```js
   window.addEventListener('message', (e) => {
     if (e.data?.type === '__activate_edit_mode') { /* show Tweaks panel */ }
     if (e.data?.type === '__deactivate_edit_mode') { /* hide it */ }
   });
   ```
2. **Then** call: `window.parent.postMessage({type: '__edit_mode_available'}, '*');`
   This makes the toolbar toggle appear.

3. When a value changes, apply it live **and** persist:
   ```js
   window.parent.postMessage({type: '__edit_mode_set_keys', edits: {fontSize: 18}}, '*');
   ```

## Persisting State

Wrap tweakable defaults in comment markers:

```js
const TWEAK_DEFAULTS = /*EDITMODE-BEGIN*/{
  "primaryColor": "#D97757",
  "fontSize": 16,
  "dark": false
}/*EDITMODE-END*/;
```

The block between markers **must be valid JSON** (double-quoted keys and strings). Exactly one such block in the root HTML file, inside inline `<script>`.

## Tips

- Keep the Tweaks surface small — a floating panel in the bottom-right, or inline handles
- Hide controls entirely when Tweaks is off; the design should look final
- If the user asks for multiple variants of a single element, use tweaks to cycle through options
- Even if the user doesn't ask for tweaks, add a couple by default — expose interesting possibilities
- Title your panel **"Tweaks"** so the naming matches the toolbar toggle
