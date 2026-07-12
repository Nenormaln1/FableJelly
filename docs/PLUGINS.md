# FableJelly × Plugins

FableJelly is built to make plugin UI feel native. Compatibility comes in two layers.

## Layer 1 — automatic inheritance

Jellyfin plugins overwhelmingly build their UI from Jellyfin's own primitives: `emby-button`, `emby-input`, `emby-select`, `emby-checkbox`, `paper-icon-button-light`, `listItem`, `cardBox`/`cardScalable`, `formDialog`, `actionSheet`, `dialog`, `toast`, `verticalSection`/`sectionTitle`.

FableJelly themes those primitives directly — with design tokens, not hardcoded colours — so any plugin that uses them is themed **the moment it renders**, including its settings pages in the dashboard. This covers, among others:

- **Home Screen Sections** (IAmParadox27) — sections and cards are stock primitives
- **Trickplay / Jellyscrub** — scrubbing previews ride the themed slider + `sliderBubble`
- **Collection/playlist tools, metadata editors, Skin Manager, File Transformation** — dialog- and form-based UI

Two defensive catch-alls extend the net to plugins that inject raw elements:

- Any `button` whose class or id contains "skip" inside the video player becomes a FableJelly glass pill (gradient on hover)
- Any badge/tag element injected over card artwork picks up glass-chip styling

## Layer 2 — bespoke integrations

### Media Bar — [MakD/Jellyfin-Media-Bar](https://github.com/MakD/Jellyfin-Media-Bar)

The deepest integration in the theme. FableJelly never fights the plugin's layout JS — it reskins every visual atom instead:

| Element | Treatment |
| --- | --- |
| Play button | Signature gradient pill with glow, spring hover |
| Details / favourite buttons | Frosted glass circles with hairline borders |
| Slide dots | Elongate into gradient progress pills when active |
| Genre row | Display-face micro-caps with ember separators |
| Plot | Left-aligned, 3-line clamp, readable measure (58ch) |
| Age rating | Glass chip instead of a solid white block |
| Loading veil | Canvas-coloured with a gradient progress bar |
| Arrows / pause / volume | Quiet glass, awake on hover; hidden on TV |
| Title logo | Cinematic drop shadow |

The bar's bottom mask fades into FableJelly's canvas automatically, so the hero blends into the aurora background with no seam.

Compatible forks that keep MakD's DOM (IAmParadox27's plugin packaging, media-bar-enhanced, etc.) get the same treatment — the selectors target the shared class names (`#slides-container`, `.play-button`, `.dots-container`…).

### Jellyfin Enhanced — [n00bcodr/Jellyfin-Enhanced](https://github.com/n00bcodr/Jellyfin-Enhanced)

| Feature | Treatment |
| --- | --- |
| Quality tags (`.quality-overlay-label`) | Glass chips; resolution gets an iris keyline, HDR/DV formats an ember one |
| Player OSD ratings (`#je-osd-rating-container`) | Tomato/TMDB chips become glass pills; stars take the warn tone |
| Smart bookmarks | Timeline markers glow ember; the bookmark modal, inputs and buttons are full FableJelly glass |
| Pause screen | Progress bar takes the gradient; plot and metadata set in the theme's type |
| Seerr integration | Request buttons take the gradient; pending/error states use the warn/danger tokens; discovery cards get a dashed accent frame |
| Maintenance banner | Warn-tinted glass |
| Settings panel & hotkey sheet | Inherit via Layer 1 (dialog/form primitives) |

> [!NOTE]
> Keep Jellyfin Enhanced's **theme selector on "Default"**. FableJelly is applied through Custom CSS; selecting another theme there stacks two themes and produces mixed styling.

### Intro Skipper / Media Segments — [intro-skipper/intro-skipper](https://github.com/intro-skipper/intro-skipper)

Skip buttons (any variant: the native 10.10+ `button.skip-button`, legacy `#skipIntro`, `btnSkipSegment`, and future lookalikes via the `[class*="skip"]` net) render as floating glass pills that fill with the gradient on hover/focus. The plugins' own fade-in/out timing is preserved.

### Media Bar as a plugin — [IAmParadox27/jellyfin-plugin-media-bar](https://github.com/IAmParadox27/jellyfin-plugin-media-bar)

The plugin packaging (2.x) ships MakD's implementation unchanged — verified selector-for-selector against 2.4.x — so it gets the full bespoke treatment above with no extra setup.

### Custom Tabs — [IAmParadox27/jellyfin-plugin-custom-tabs](https://github.com/IAmParadox27/jellyfin-plugin-custom-tabs)

The tab buttons themselves ride the themed tab strip automatically. Tab *content* is HTML you author; build it from Jellyfin primitives (`verticalSection`, `sectionTitle`, `cardBox`, `listItem`, `emby-button`) and it will look native in FableJelly for free. All `--fj-*` tokens are available to your custom CSS too.

### Server-side plugins (no UI)

AudioDB, MusicBrainz, OMDb, TMDb, Open Subtitles, Studio Images, Titlovi and similar metadata/scraper plugins render no web UI — their settings pages inherit via Layer 1. **File Transformation** is infrastructure other plugins use; nothing to theme.

## Requesting deeper support

Open an issue with:

1. The plugin name + repo link
2. A screenshot of the rough edge
3. If you can: the injected element's class/id (right-click → Inspect)

Bespoke rules live in sections 21–23 of `fablejelly.css`, each mapped to the plugin's public DOM — pull requests welcome.
