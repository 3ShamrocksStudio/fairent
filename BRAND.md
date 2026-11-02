# Fairent · הוגן — Brand

> Power back to renters. Fair rent, verified by the community.
> A 3Shamrocks Studio product, built on the **3S Design System v3.0**.

## The 4 brand colors (extracted from the real asset)

These are the **exact** hexes used in the official vector artwork
(`Brand/Fairent_icon.svg` and `Brand/Fairent_icon_logo.svg`, both exported from
`Brand/Fairent.ai`). They are the single source of truth for the Fairent brand
world and are mapped onto the 3S Design System semantic roles in the
`[data-brand="fairent"]` Layer‑1 block inside `index.html`.

| # | Hex | Name | Role in the brand | DS mapping |
|---|-----|------|-------------------|------------|
| 1 | `#310A91` | **Deep Indigo** | Primary — justice / trust ("RENT") | `--brand-700`, neutral `--tint`, light `theme-color` |
| 2 | `#6550FF` | **Electric Violet** | Secondary — energy / action | `--brand-500`, dark‑mode `--accent` |
| 3 | `#53D805` | **Signal Green** | Signature — "the home" / fair | `--signature-500` |
| 4 | `#050511` | **Ink Black** | Depth — text, dark canvas | `--brand-950`, dark `theme-color`, wordmark ink |

```
#310A91   Deep Indigo     ████████
#6550FF   Electric Violet ████████
#53D805   Signal Green    ████████
#050511   Ink Black       ████████
```

### Provenance

Extracted directly from the SVG `<style>` fills shipped in the artwork:

```
Brand/Fairent_icon.svg        .cls-1 #310a91 · .cls-2 #6550ff · .cls-3 #53d805
Brand/Fairent_icon_logo.svg   .cls-1 #310a91 · .cls-2 #6550ff · .cls-3 #050511 · .cls-4 #53d805
```

No colors were invented. The ramp tints/shades around `#6550FF`/`#310A91`/`#050511`
are generated for UI states (hover, soft fills, borders) but every brand anchor
above appears verbatim.

## Logo assets

| File | Use |
|------|-----|
| `Brand/Fairent_icon_logo.svg` | Full lockup (icon + "Fairent" wordmark) — header & footer, on a white chip so it reads in both light and dark modes |
| `Brand/Fairent_icon.svg` | Icon mark alone — favicon (SVG), app watermark |
| `Brand/favicon-32.png` · `Brand/icon-180.png` · `Brand/icon-512.png` | PWA / touch icons |

## Type

- **Display / headings:** Heebo (Hebrew‑first, 700–900)
- **Body:** Assistant, falling back to Heebo
- Hebrew‑first, RTL (`dir="rtl"`, `lang="he"`), with Latin chrome pinned LTR.

## Identity v2 (2 Nov 2026)

- **Mood:** "power back to renters". Night-city hero in Ink → Indigo → Violet, with Signal
  Green as the one "fair" colour (CTAs, the fair end of the dial, positive numbers).
- **Logo use:** full lockup on light surfaces as-is; on dark surfaces always on a white chip
  (the indigo "RENT" must never sit on indigo). Never recoloured.
- **Type:** Heebo 900 for headlines and every number (tabular figures), Assistant for body.
  Hebrew numbers and ranges are bidi-isolated so `+5.3%` and `2.5–3` read correctly in RTL.
- **Icon set:** `Brand/fairent-icons.svg` — 21 icons on a 24px grid, 2px round strokes in
  `currentColor`, with one Signal Green accent stroke per icon (class `g`). Inlined as a sprite
  in `index.html`. No emoji in the UI.
- **Imagery:** an inline SVG skyline (brand ramps only, lit windows in Signal Green/violet) with
  the real Fairent icon on the rooftop sign, plus floating cards that carry **real CBS numbers**.
  No stock photos, no invented figures.
- **Psychology:** lead with the renter's money (yearly overpay in ₪), show the source next to
  every number, and end with an action (copy / WhatsApp the negotiation message).

## Design‑system compliance

Fairent inherits 3S Design System v3.0 verbatim (`tokens.css` + `components.css`,
inlined for the single‑file PWA) and ships exactly **one** Layer‑1 brand block
(`[data-brand="fairent"]`). It targets the COMPLIANCE.md gate (`ds-audit.py`
score ≥ 90): semantic tokens only, real studio logo + `© 2026 3Shamrocks Studio`
credit, visible focus rings, ≥44px tap targets, `prefers-reduced-motion`
support, and full RTL.

© 2026 3Shamrocks Studio. All rights reserved.
