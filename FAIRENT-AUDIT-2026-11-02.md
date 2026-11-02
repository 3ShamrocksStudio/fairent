# Fairent (הוגן) — product, business model and next-steps audit

2 Nov 2026 · prepared for Dave (CEO) · status: **staging only, not launched** · parked in the studio brain until 21 Dec 2026.
Every fact below has a link. Anything that is our estimate or a proposal says so.

---

## 1. What Fairent is today (after the 2 Nov upgrade)

A free, Hebrew-first web app at https://3shamrocksstudio.github.io/fairent/ (staging, `noindex`):

1. Renter enters city, rooms and asking rent (address and size optional).
2. The app compares it with **average rent actually paid** in that city and room band, from the
   CBS Rent Survey ([Table 4.9, Q2 2025](https://www.cbs.gov.il/he/publications/Madad/DocLib/2025/price09a/a4_9_e.pdf)),
   updated for new leases with the CBS September 2026 change for new tenants
   (+5.3%, renewals +2.4%; [report of the CBS release](https://www.nadlancenter.co.il/article/15581)).
3. Result: % above or below the estimate, yearly overpay in ₪, comparison bars (city, district, national,
   all CBS), a fair range, an OSM map of the address, and a negotiation message in two tones
   (copy or open in WhatsApp).
4. A city explorer shows the real CBS numbers for 18 big cities by room band.
5. A rights section summarises the 2017 Fair Rent Law (deposit cap, repair deadlines, notice periods,
   broker fees) with sources.

**Removed in this pass because they were not real:** three sample "renter reports" for Dizengoff 99,
"similar apartments nearby" (they were modelled, not real listings), the "verified by the community"
claim, and the personal Gmail in the footer. Renter reports are shown as "not open yet".

**Known limits (stated in the app):** CBS publishes city-level averages only (no street or building);
the Q2 2025 level table is the newest one we could verify from the CBS source; the +5.3% new-lease
update and the ±5% size tweak are our method, labelled as estimates.

---

## 2. Users

| Segment | Size (sourced) | Pain | Moment of use |
|---|---|---|---|
| Renters signing a **new** lease | 29.6% of Israelis lived in rented homes in 2025 ([CBS via Globes, Mar 2026](https://en.globes.co.il/en/article-1001537709)); ~851k rented dwellings, 28.8% of stock, Jul 2024 ([CBS via Nadlan Center](https://www.nadlancenter.co.il/article/11770)) | No public record of what others pay; asking prices are the only reference | The 1–3 days between viewing and signing |
| Renters renewing | same pool; renewals +2.4% vs new tenants +5.3% ([source](https://www.nadlancenter.co.il/article/15581)) | Landlord asks for a raise at renewal | 60–90 days before lease end (notice periods per the [2017 law](https://timeout.co.il/%D7%97%D7%95%D7%A7-%D7%94%D7%A9%D7%9B%D7%99%D7%A8%D7%95%D7%AA-%D7%94%D7%94%D7%95%D7%92%D7%A0%D7%AA/)) |
| New immigrants, students, relocating employees | not sized yet | Don't know the market at all | Before arrival / first lease |

About a quarter of renters are new tenants each year (Bank of Israel estimate, [BoI 2019](https://boi.org.il/media/yulnw1sl/part-3n.pdf)).
That is the core audience: people who face market prices, not renewal prices.

## 3. Value proposition

**"Know what this apartment really costs before you sign, and walk in with a message that says so."**

Why it is a real edge, not a nice-to-have:
- The official rent index understates what new tenants face: 2008–2015 actual average rent rose 59%,
  the index only 34% ([Bank of Israel](https://boi.org.il/media/yulnw1sl/part-3n.pdf)). Renters who read
  "rent rose 2%" in the news are being anchored low; landlords are not.
- Listing sites show **asking** prices. Fairent shows **paid** prices (CBS survey) and says so on every number.
- It ends in an action (a negotiation message with the source inside), which is what changes the price.

## 4. Competitors and substitutes

| Player | What it does | Data | Rent fairness check? | Notes |
|---|---|---|---|---|
| Yad2 | Largest listings site | Asking prices | No | EULA bans scrapers ([EULA](https://www.2.yad2.co.il/en/eula)); co-owns WeCheck |
| Madlan | Listings + area analytics | Listing-derived (asking) | Partial (area medians) | No public API (see DATA-STRATEGY.md) |
| Realta | Aggregates listings from Yad2, Madlan, OnMap, Facebook etc.; rent stats by city/rooms | Asking, aggregated daily | No fairness tool or negotiation help | [realta.co.il/en/market](https://realta.co.il/en/market) — closest content competitor |
| WeCheck | Tenant screening + rent guarantee for landlords | Private | No — landlord-side | Jointly owned by Yad2 + Isracard, founded 2017 ([Startupim](https://startupim.com/company/wecheck)) |
| nadlan.gov.il | Official sale transactions | Actual **sale** prices | No rent | [gov.il](https://www.gov.il/en/service/real_estate_information) |
| CBS tables | The raw source | Actual paid rent, city level | No | PDF tables, not consumer-friendly |

**Position:** the only renter-side tool that (a) uses paid-rent data, (b) cites it, and (c) turns it into a
negotiation step. Weakness: no street/building granularity until renter reports exist.

## 5. Business model — audit

Dave prefers recurring revenue. Honest read of each option (all prices are **proposals for Dave**, not decided):

| Model | Recurring? | Who pays | Fit | Verdict |
|---|---|---|---|---|
| A. Free for renters (mission + reach) | — | nobody | Needed for trust and data | **Keep.** Never paywall the basic check. |
| B. Renter subscription ("Fairent Plus") | Yes | renters | Weak: renters need it once per 1–3 years | **Drop as a lead model.** At most a one-time paid report later. |
| C. Embeddable widget / data licence for media and consumer sites (housing sections, relocation blogs) | Yes, monthly licence | publishers | Good: content they lack, cheap for us to serve | **Test first.** Hypothesis: small monthly licence per site. NEEDS YOUR EYES on price. |
| D. Employer relocation / HR benefit | Yes, annual | companies relocating staff | Medium: real budget, long sales cycle | Test after C. |
| E. Partner referrals (movers, renter insurance, deposit-guarantee products) | Per lead | partners | Medium; must never bias the verdict | Later, with a clear "partner" label. |
| F. Municipal / NGO housing desks | Yes | councils, renter-rights NGOs | Medium; overlaps SH✡MER buyer list (councils) | Later; reuse SH✡MER council relationships. |

**Against the $1M by 21 Apr 2027 goal:** Fairent will not move M1–M3. Expected cash before 21 Jan 2027 ≈ 0
(our estimate: no buyer conversation exists yet). Its near-term value is (1) a strong studio case study for
the services pipeline and (2) one cheap licence test (C). Recommendation: **stay parked for build work,
allow one revenue test**, revisit 21 Dec as the brain already says.

**Kill-test for C (cheapest disproof):** 10 targeted publisher conversations → if zero say "send a trial",
drop C and keep Fairent as a portfolio piece only.

## 6. Pricing (proposal only — Dave decides)

- Renters: free. No ads inside the result.
- Widget licence (C): flat monthly fee per site, includes the CBS-backed checker in their brand colours,
  quarterly data refresh, and a source line. Price to be set by Dave after the first 3 conversations;
  no number goes into any first contact (BRAIN rule 8).

## 7. Next steps (dated)

| # | Step | Owner | Due | Done when |
|---|---|---|---|---|
| 1 | Dave reviews the staged app and studio case page (NEEDS YOUR EYES) | Dave | 4 Nov 2026 | Go / changes logged |
| 2 | Pull the newest CBS Table 4.9 (Q2 2026 or later) from an Israeli connection and replace the Q2 2025 values; keep the source link | CTO | 15 Nov 2026 | New period in the UI + DATA-STRATEGY.md |
| 3 | Each month, update the new-lease % from the CBS CPI release (next: 15 Nov 2026) | CTO | monthly | Constant + source link updated |
| 4 | Build the shared, k-anonymised renter-report backend on a free tier (≥5 reports per cell, no names, street-level jitter) and re-open reports | CTO + CLO | 21 Dec 2026 | Privacy check passed, Dave OK |
| 5 | List 20 Hebrew housing/relocation publishers + named editors (sources) for test C | CRO | 30 Nov 2026 | List in Dave's private to-do |
| 6 | Draft the first-contact note for C (no price) for Dave's approval | CMO | 3 Dec 2026 | Draft in to-do, nothing sent |
| 7 | Add "what did you end up paying?" after the message is copied (feeds step 4) | CPO | 21 Dec 2026 | Live on staging |
| 8 | Free trademark check for "Fairent" and "הוגן" | CLO | 15 Dec 2026 | Result logged |

## 8. Sources used
- CBS Table 4.9, Q2 2025: https://www.cbs.gov.il/he/publications/Madad/DocLib/2025/price09a/a4_9_e.pdf
- CBS CPI July 2026 release (new tenants +4.7%, renewals +2.6%): https://www.cbs.gov.il/he/mediarelease/Madad/DocLib/2026/257/10_26_257b.pdf
- CBS CPI September 2026 (new tenants +5.3%, renewals +2.4%), via Nadlan Center: https://www.nadlancenter.co.il/article/15581
- Renter share 2025 (29.6%): https://en.globes.co.il/en/article-1001537709
- Rented dwellings July 2024: https://www.nadlancenter.co.il/article/11770
- Bank of Israel on rent index bias: https://boi.org.il/media/yulnw1sl/part-3n.pdf
- Fair Rent Law 2017 summary: https://timeout.co.il/%D7%97%D7%95%D7%A7-%D7%94%D7%A9%D7%9B%D7%99%D7%A8%D7%95%D7%AA-%D7%94%D7%94%D7%95%D7%92%D7%A0%D7%AA/
- Broker fees in rentals: https://www.d.co.il/Article10885/
- Realta: https://realta.co.il/en/market · WeCheck: https://startupim.com/company/wecheck · Yad2 EULA: https://www.2.yad2.co.il/en/eula
