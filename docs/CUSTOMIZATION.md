# Customizing FableJelly

Every visual decision in FableJelly flows through a design token — a CSS custom property with the `--fj-` prefix. Override any of them **after** the core import (Custom CSS is plain CSS: later rules win at equal specificity):

```css
@import url("https://cdn.jsdelivr.net/gh/Nenormaln1/FableJelly@latest/fablejelly.css");

:root {
    --fj-accent: 0, 200, 255;
}
```

> **Colour format:** colour tokens are raw `R, G, B` triplets (no `rgb()` wrapper). This lets the theme build transparency variants of your colour: `rgba(var(--fj-accent), 0.25)`. Convert any hex colour at [rgbcolorcode.com](https://www.rgbcolorcode.com/).

---

## Token reference

### Canvas

| Token | Default | Role |
| --- | --- | --- |
| `--fj-bg` | `9, 10, 16` | Page background |
| `--fj-bg-elevated` | `15, 17, 26` | Raised panels (TV dialogs, selects) |
| `--fj-surface` | `21, 24, 36` | Cards, inputs, menus |
| `--fj-surface-2` | `30, 34, 50` | Hovered/active surfaces, chips |
| `--fj-line` | `148, 158, 190` | Hairline borders (used at low alpha) |

### Ink (text)

| Token | Default | Role |
| --- | --- | --- |
| `--fj-ink` | `236, 239, 248` | Primary text |
| `--fj-ink-dim` | `165, 172, 195` | Secondary text |
| `--fj-ink-faint` | `110, 117, 140` | Tertiary text, placeholders |

### Accent duo

| Token | Default | Role |
| --- | --- | --- |
| `--fj-accent` | `139, 125, 255` | First accent (iris) |
| `--fj-accent-2` | `255, 156, 110` | Second accent (ember peach) |
| `--fj-on-accent` | `16, 13, 30` | Text placed on accent surfaces |
| `--fj-gradient-angle` | `118deg` | Direction of the signature gradient |
| `--fj-gradient` | *(derived)* | The full gradient — override to replace it entirely |

### Signals

| Token | Default | Role |
| --- | --- | --- |
| `--fj-ok` | `74, 222, 154` | Success |
| `--fj-warn` | `255, 199, 89` | Warnings, star ratings, transcoding |
| `--fj-danger` | `255, 105, 125` | Errors, delete buttons |

### Glass

| Token | Default | Role |
| --- | --- | --- |
| `--fj-glass` | `13, 15, 24` | Glass tint colour |
| `--fj-glass-alpha` | `0.62` | Glass opacity |
| `--fj-glass-blur` | `22px` | Backdrop blur radius |
| `--fj-glass-border` | `rgba(…, 0.14)` | Glass edge hairline (full CSS value) |

### Shape

| Token | Default | Role |
| --- | --- | --- |
| `--fj-radius-xs` | `8px` | Small images, focus outlines |
| `--fj-radius-sm` | `12px` | Inputs, list rows, menu items |
| `--fj-radius` | `16px` | Cards |
| `--fj-radius-lg` | `24px` | Dialogs, OSD island, login card |
| `--fj-radius-pill` | `999px` | Buttons, chips, tabs |

### Type

| Token | Default | Role |
| --- | --- | --- |
| `--fj-font-display` | `"Sora", …` | Titles, section headings, dialog headers |
| `--fj-font-body` | `"Figtree", …` | Everything else |

The core also loads **Onest** as a per-glyph fallback: any character Sora/Figtree don't cover (Cyrillic, extended Latin) renders in Onest instead of a system font, so non-English libraries stay in the theme's voice.

The easiest way to change fonts is a ready-made pack from [`fonts/`](../fonts) (onest, inter, manrope, outfit, space-grotesk, system) — one import, both variables set, script coverage documented in each file. For anything else, add your own `@import` above the overrides:

```css
@import url("https://cdn.jsdelivr.net/gh/Nenormaln1/FableJelly@latest/fablejelly.css");
@import url("https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400..700&display=swap");

:root {
    --fj-font-display: "Space Grotesk", sans-serif;
}
```

### Motion

| Token | Default | Role |
| --- | --- | --- |
| `--fj-t-fast` | `140ms` | Micro-interactions (hover tints) |
| `--fj-t` | `260ms` | Standard transitions |
| `--fj-t-slow` | `480ms` | Entrances, art zoom |
| `--fj-ease` | `cubic-bezier(0.22, 1, 0.36, 1)` | Default easing |
| `--fj-ease-spring` | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Playful overshoot (lifts, scales) |

### Ambience

| Token | Default | Role |
| --- | --- | --- |
| `--fj-aurora-opacity` | `0.5` | Aurora strength (`0` = off) |
| `--fj-aurora-drift` | `90s` | One drift cycle |
| `--fj-grain-opacity` | `0` | Film grain (the grain addon sets `0.05`) |

### Cards

| Token | Default | Role |
| --- | --- | --- |
| `--fj-card-lift` | `-6px` | Hover rise distance |
| `--fj-card-zoom` | `1.05` | Hover art zoom |
| `--fj-glow-alpha` | `0.30` | Hover glow strength (`0` = off) |

---

## Recipes

### One flat accent instead of a gradient

```css
:root {
    --fj-accent: 229, 9, 20;
    --fj-accent-2: 229, 9, 20;   /* same colour = flat */
    --fj-on-accent: 255, 255, 255;
}
```

### Calm mode — no glow, no aurora, slower motion

```css
:root {
    --fj-glow-alpha: 0;
    --fj-aurora-opacity: 0;
    --fj-card-lift: -2px;
    --fj-card-zoom: 1.01;
}
```

### Frosted maximalism

```css
:root {
    --fj-glass-blur: 36px;
    --fj-glass-alpha: 0.45;
    --fj-aurora-opacity: 0.85;
}
```

### Build your own preset

Copy [`presets/nebula.css`](../presets/nebula.css), change the numbers, and host it anywhere (a GitHub gist works). Import it after the core — that's all a preset is.

### Per-user theming

Each Jellyfin account can have its own Custom CSS (**Settings → Display**). Server-wide CSS applies first, per-user CSS after — so users can layer their own preset over the server's FableJelly install.

---

## How overrides interact

Import order matters. The safe order is:

```css
/* 1. core */
@import url(".../fablejelly.css");
/* 2. preset (token overrides) */
@import url(".../presets/ocean.css");
/* 3. addons (behaviour overrides) */
@import url(".../addons/grain.css");
/* 4. your own :root / rule overrides last */
:root { --fj-radius: 20px; }
```

All `@import` lines must come before any plain rules — that's a CSS requirement, not a FableJelly one.
