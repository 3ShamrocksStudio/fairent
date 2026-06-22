# Fairent (הוגן) — Data Strategy

> The make-or-break question for this product is **not** the UI. It is: *where does real
> Israeli rent data legally come from, given Israel has no public rent registry?*
> This document answers that concretely, honestly, and with sources. Read this first.

**TL;DR verdict:** It is **viable, but only as a "fairness band," not a per-address "you're
overcharged by exactly X%" oracle.** There is no legal, address-level *actual-paid-rent* source
in Israel — and, by a deliberate 2025 legislative choice, there never will be a government one.
The realistic v1 is a **three-layer stack**: (1) **CBS actual-rent averages** as the legal,
free, day-one core; (2) **crowdsourced renter contributions** as the moat that adds
neighborhood/building granularity over time; (3) an **optional, risk-managed asking-price
signal** that we do *not* build the company on. The cold-start works on day one using CBS
city × room-band data presented as a *range with provenance*.

---

## 1. The decisive context: no registry exists, and none is coming

Israel's **Fair Rent Law (חוק שכירות הוגנת)** passed in 2025 — but the clauses that would have
helped us most were **stripped before passage**: price controls, a 3-year rent cap, a rental
commissioner, **and mandatory disclosure of prior contracts so renters could compare rents.**
What survived is habitability standards and a ban on landlords pushing broker fees onto tenants.

**Implication:** the state will *not* create the comparable-rent registry this product wants.
"Wait for open government rent data" is permanently off the table. A proxy + crowdsource
approach isn't a compromise — it's the only path that exists.
→ https://www.calcalist.co.il/local/articles/0,7340,L-3717369,00.html
→ https://www.shibolet.com/התיקון-לחוק-שכירות-הוגנת/

---

## 2. Source-by-source findings

### ✅ CBS — Central Bureau of Statistics — *the legal core*
- **Dataset:** "Average Monthly Prices of Rent" (a distinct page from the sale-price / dwelling
  indices). → https://www.cbs.gov.il/en/subjects/Pages/Average-Monthly-Prices-of-Rent.aspx
- **Granularity:** by **city** (population > 100k), by **district** (North, Haifa, Central,
  Tel Aviv, Jerusalem, South), **and by room-count band** (1–2, 2.5–3, 3.5–4, 4.5–6 rooms).
  Quarterly.
- **Asking or actual?** **ACTUAL paid rent**, from the CBS **Rent Survey** (סקר שכר דירה), a
  longitudinal panel of real renters. This is the key differentiator vs. any listings site.
- **Access:** official, free, sanctioned **CBS API** (Time-Series DataBank / Price Indices /
  SDMX); only a `User-Agent` header required, no key.
  → https://www.cbs.gov.il/en/Pages/Api-interface.aspx
- **⚠️ Two caveats that matter:**
  1. **Granularity stops at city/district** — not neighborhood, not street, not building.
  2. **Use the average-rent *levels* tables, NOT the monthly Rental Price Index.** The index
     uses a *repeat-rent (renewals)* methodology that largely excludes new tenants and so
     **materially understates** what someone signing a *new* lease faces. Bank of Israel:
     2007–2015 the index rose ~34% while average actual rents rose ~59% — roughly double. New
     tenants (~25% of the market) see steeper jumps.
     → https://www.boi.org.il/media/yulnw1sl/part-3n.pdf
- **Open action item:** confirm whether the average-rent *level* tables are served via the API
  or are **Excel-only download** from the subject page. (The *index* is API-served; the *levels*
  may be Excel.) Confirm before architecting the ingest.

### ✅ nadlan.gov.il (Tax Authority real-estate DB) — *sale prices only, useful as context*
- Address-level **SALE** transactions (actual prices from capital-gains filings). **No rentals**
  — rent isn't reported to the Tax Authority. Useful as a *price-per-sqm location-quality proxy*
  and price-to-rent context, not as a rent source.
  → https://www.gov.il/en/service/real_estate_information

### ✅ Arnona (municipal property-tax) zones — *cheap legal location proxy*
- Municipalities publish arnona rates by zone; zone correlates with desirability. A legal,
  public **relative** multiplier to nudge a city average up/down for neighborhood quality.
  No single national dataset — assembled per-municipality, often PDF. Use as a *soft* signal.

### ⚠️ Yad2 (yad2.co.il) — *do NOT build on this*
- No public API. **EULA explicitly bans scrapers by name** ("…not to collect data… including by
  any… automated software (including robots, spiders, scrapers, etc.)").
  → https://www.2.yad2.co.il/en/eula
- robots.txt disallows `/api`, `/ajax`, and the real-estate price/filter params.
  → https://www.yad2.co.il/robots.txt
- **Asking prices only** (advertised, systematically above achieved rent).
- Commercial-grade anti-bot (PerimeterX/HUMAN/Cloudflare/DataDome class), changes often.
- **Actively litigating against rival Keyz right now** over exactly this — unauthorized
  collection/republication of listings. Litigation appetite is demonstrated, precedent unsettled.
  → https://www.onlinemarketplaces.com/articles/yad2-and-keyz-clash-over-tv-advert-and-listings-data-in-israel/
- **Verdict:** explicit contractual ban + live litigation = real, not theoretical, risk. Not a backbone.

### ⚠️ Madlan (madlan.co.il) — *same posture as Yad2*
- Covers sales **and** rentals, analytics-heavy (neighborhood medians, ₪/sqm by room count).
  No official public API — only unsanctioned third-party Apify scrapers. Rent figures are
  listing-derived (**asking**). ToS page 301-redirected (unverified) but assume standard
  anti-scraping. Useful as a benchmark *you eyeball*, not a pipeline you depend on.

### ⚠️ WeCheck (wecheck.co.il) — *competitor, not a source*
- Tenant-screening / rent-guarantee, **jointly owned by Yad2 + Isracard.** Holds rich rental
  data internally but is Yad2-adjacent. Treat as competitive landscape, not a data source.

### Bank of Israel
- Publishes the housing/rent analysis and the new-vs-renewal critique above, but **relies on the
  same CBS rent survey**. Use BoI for the **new-tenant correction factor**, not as a feed.

---

## 3. Legality of scraping in Israel (summary)
- No statute flatly bans scraping; Israeli courts lean toward allowing scraping of **public**
  data (**Bright Data v. X dismissed in Israel**), and have held ToS can't manufacture IP rights
  beyond statutory copyright.
  → https://www.law.co.il/news/2024/05/15/court-dismisses-x-scraping-lawsuit-against-brightdata/
- **But three real exposures remain:** (a) **contract** — Yad2's EULA names scrapers explicitly;
  (b) **active litigation** (Yad2 v. Keyz) over the identical act; (c) **privacy** — the Privacy
  Protection Authority's 2025 guidance says scraping can violate privacy law even from public
  sources, and **Amendment 13** (in force ~Aug 2025) adds GDPR-style enforcement teeth; listing
  data includes contact details = personal data.
  → https://iapp.org/news/a/israel-marks-a-new-era-in-privacy-law-amendment-13-ushers-in-sweeping-reform
- **Practical read:** scraping *aggregate, non-personal asking prices* sits in a gray-but-defensible
  zone; doing it against an explicit EULA from a litigious incumbent is a real risk. Routing it
  through Apify/Bright Data just relocates the risk to a vendor — it doesn't legalize it.

## 4. Crowdsourcing actual paid rent — *the moat* (legal if done right)
This is the **only** path to address/building-level *actual* rent, and it's legal under
**Amendment 13** if we:
- Take **informed consent + purpose notice** at collection.
- **Aggregate/anonymize before display** — enforce **k-anonymity** (never show a cell backed by
  < ~5 contributions; jitter exact addresses to street/block).
- Store contributions **pseudonymously** — rent + street + size is generally *not* special-
  sensitivity data, but tying it to an identifiable person would make it personal data, so strip
  identity.
- (Registration/notification duties kick in only at scale — controller of >100k individuals.)

**Weakness:** cold-start — worthless on day one. Solved below.

---

## 5. Recommended v1 data architecture

```
                 ┌─────────────────────────────────────────────┐
   LEGAL CORE →  │ CBS average ACTUAL rent  (city × room-band)  │  free · official API · day one
   (anchor)      │ + BoI new-tenant uplift correction          │
                 └───────────────────┬─────────────────────────┘
                                     │  base fair-rent estimate
   LOCATION   →  ┌───────────────────┴─────────────────────────┐
   ADJUST        │ nadlan ₪/sqm sale proxy · arnona zone        │  relative multiplier (disclosed proxy)
                 └───────────────────┬─────────────────────────┘
                                     │  neighborhood-adjusted band
   MOAT       →  ┌───────────────────┴─────────────────────────┐
   (grows)       │ Crowdsourced ACTUAL rent (k-anonymized)      │  blends in, raises confidence + granularity
                 └───────────────────┬─────────────────────────┘
                                     │
                 ┌───────────────────┴─────────────────────────┐
   OPTIONAL   →  │ Asking-price signal — user-pasted comps OR   │  labeled "asking, not paid", discounted
   (careful)     │ only clearly-permissible ingest. NOT Yad2.   │  NEVER the backbone
                 └─────────────────────────────────────────────┘
```

**Output contract:** always present a **range with provenance**, e.g.
*"Based on CBS Q3-2025 actual rents for 3-room flats in your city (₪X), adjusted for your
neighborhood, a fair range is ₪A–₪B. Your ₪Y is N% above the top of that range."* Cite CBS inline.
Never lead with a confident per-address "overcharged by exactly 18%" — we can't back it on day
one and it invites both inaccuracy and landlord pushback.

## 6. Cold-start plan (zero crowdsourced data) — it works
1. **Base** = CBS average actual rent for the user's **city × room-band** (district fallback for
   towns < 100k). Legal, actual, citable.
2. **Location adjust** = arnona zone and/or nadlan ₪/sqm as a *relative* multiplier (disclosed proxy).
3. **New-lease correction** = apply the BoI-documented new-tenant uplift so we don't understate
   what a new renter faces.
4. **Present as a band with the CBS citation visible.**
5. **Convert every user into a contributor** — the moment we hand over the negotiation script is
   the natural time to ask *"What did you end up paying?"* That fills the crowdsource layer and
   turns the proxy into real comparables within months.

---

## 7. What the current PoC does vs. this strategy
The PoC (`index.html`) implements the **shape** of this architecture with a **transparent
heuristic over seed proxy data** so the flow is real and demonstrable today:
- **Seed benchmark** = a hand-set `₪/m²/month` table by city (realistic order-of-magnitude
  placeholders, clearly labeled "seed" in the UI). **To be replaced by the CBS city × room-band
  feed** — that is the single most important next data task.
- **Crowdsource layer is live in miniature**: contributions are stored locally (localStorage),
  blended into the city benchmark, and raise the on-screen confidence meter — proving the
  flywheel end-to-end. Production swaps localStorage for a k-anonymized backend.
- **Honest framing throughout**: every result shows its data provenance and a "not a certified
  appraisal" disclaimer; the methodology is spelled out in an in-app "שקיפות מלאה" panel.

## 8. Open action items (verify before scaling past PoC)
- [x] **Confirm CBS average-rent levels are API-served vs. Excel-only.** → **PDF-only.** The level
  table is **Table 4.9** (PDF); the CBS API serves only the rent *price index* (2024=100), not
  shekel levels. Ingest = parse the quarterly PDF, not the API.
  English file: `https://www.cbs.gov.il/he/publications/Madad/DocLib/2025/price09a/a4_9_e.pdf`
- [x] **Pull the exact CBS room-band × city table and replace the seed `₪/m²` numbers.** → **Done.**
  `index.html` now ships the real CBS **Q2 2025** table — national + all 6 districts + 18 big
  cities, each × 4 room-bands (1–2 / 2.5–3 / 3.5–4 / 4.5–6). Values extracted from `a4_9_e.pdf`
  and **independently verified** against the primary PDF (national bands, Jerusalem/North district
  totals, full Ashkelon row) + a secondary cross-check (Q1 2025 national room-bands). Model switched
  from `₪/m²×size` to **CBS actual rent by city×band**, presented as a **range with provenance**.
  Cities CBS doesn't break out fall back to **district**, then **national**, and the UI says which.
- [x] **Quantify the BoI/CBS new-tenant uplift factor.** → CBS CPI release **017/2026** (Dec-2025
  data): lease **renewals +3.0% y/y** vs **new tenants +4.6% y/y** → **~1.6pp** new-lease premium.
  Surfaced as an honest note in the result + transparency panel (not silently inflating the figure).
- [ ] **Refresh cadence:** Q3 2025 / Q1 2026 figures are circulating (secondary-quoted, ~+3.5% y/y
  national) but no newer *primary level PDF* was confirmed. Re-pull `a4_9_*.pdf` each quarter.
- [ ] Decide the asking-price posture: user-pasted comps (safe) vs. any permissible ingest.
- [ ] Design the k-anonymized crowdsource backend (consent copy, ≥5-per-cell threshold, address jitter).
- [ ] Check whether target municipalities publish machine-readable arnona-zone polygons.

> **Data provenance note (2026-06-22):** all baked-in rent figures are CBS Table 4.9, **Q2 2025
> (Apr–Jun), actual paid rent**, NIS/month. Verified against the primary CBS PDF. The figures are
> ~1 year old (newest primary level table available) — we display the period and do **not**
> extrapolate forward. Replace when CBS publishes a newer `a4_9` cut.

## Honest viability verdict
**Build it.** The data foundation is real and legal — CBS gives genuine *actual* rent at
city × room granularity for free, which is enough for a defensible, useful day-one product, and
the crowdsource moat is both legal (Amendment 13, done right) and the durable advantage no
competitor can buy. The honest constraint is **granularity and precision**: v1 is a *fairness
band with provenance*, not a per-building oracle, and the messaging must own that. Anyone
promising address-level "actual paid rent" in Israel today is either scraping asking prices
(legally exposed) or bluffing.

---
*Research synthesized 2026-06-22. Sources inline. © 2026 3Shamrocks Studio.*
