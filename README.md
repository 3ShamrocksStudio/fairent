# הוגן · Fairent

**Israel-first, Hebrew-first tool that arms renters with data + negotiation power** so they pay
meaningfully less than asked — and pressures overpriced listings down.

> *"האם אתם משלמים יותר מדי על שכר הדירה?"* — enter address + asking rent → an AI fairness
> assessment (X% above/below market) + comparables + a ready-to-send negotiation script.
> Renters also contribute what they actually pay (anonymously), which builds the comparable set.

A 3Shamrocks Studio product. © 2026 3Shamrocks Studio. All rights reserved.

---

## Status: PoC (local preview only — not published)

This is a working proof-of-concept of the core flow, built to the 3S quality bar. It is **not**
yet public — Dave reviews first.

## Run it locally
```bash
cd "Fairent"
python3 -m http.server 4488
# open http://localhost:4488
```
Or via the Claude Code preview config: server name **`fairent`** (port 4488).

## What's here
| File | Purpose |
|------|---------|
| `index.html` | The entire PoC — single-file, Hebrew-first, RTL, light/dark, PWA. |
| **`DATA-STRATEGY.md`** | **Read first.** The make-or-break: where legal Israeli rent data comes from, with sources + honest verdict + cold-start plan. |
| `manifest.json` | PWA manifest (RTL, Hebrew). |
| `Brand/` | Real Hogen product logos (primary = scales cradling a home; shield variant). |
| `3shamrocks.png` | Studio wordmark (footer, on dark backing — never fabricated). |

## The core flow (implemented)
1. **Check** — city, rooms, size, asking rent (Hebrew form, no signup).
2. **Assess** — transparent heuristic returns a fairness verdict + % vs. market, a color gauge,
   the fair-price estimate, a sensible range, and modeled comparables.
3. **Negotiate** — a dynamic, data-backed Hebrew negotiation script, copy-to-clipboard.
4. **Contribute** — anonymous "what do you actually pay?" form feeds back into the benchmark and
   raises the on-screen confidence meter (the data flywheel, in miniature).

## How it's built (3S Design System)
Inherits the studio's **Layer-1 brand-knob** method (`tokens.css` architecture): one Fairent
brand world — deep teal-blue (trust/justice) + fair-green (the home) + gold (the scales) —
generates the whole UI. Heebo/Assistant type, WCAG-AA contrast, RTL throughout, dark mode.

## Honest limitations (see DATA-STRATEGY.md §8)
- The benchmark currently uses **clearly-labeled seed `₪/m²` proxy values**, to be replaced by
  the **CBS city × room-band actual-rent feed** — the #1 next data task.
- Crowdsourced contributions are stored in **localStorage** for the PoC; production needs a
  k-anonymized backend (Amendment-13 compliant).
- This is a **negotiation aid, not a certified appraisal** — and v1 is honestly a *fairness band
  with provenance*, not a per-address oracle. Israel has no legal address-level actual-rent source.

## Why an owned repo (not base44)
Built as a fully-owned 3S repo for control, ownership, and consistency with every other 3S
product. base44 wasn't needed for the PoC; if a concrete reason emerges (e.g. fast hosted backend
for the crowdsource layer), that's a flag-to-Dave decision, not a silent switch.
