---
name: arbitrage-engine
description: >
  Universal used car auction arbitrage analysis engine. Use this skill whenever the user
  wants to analyze, score, rank, or evaluate vehicles for auction purchase and retail resale
  in ANY city or market — including: analyzing Manheim or ADESA CSV exports, scoring
  individual VINs, reading condition report PDFs, calculating max bid prices, building ranked
  buy lists, evaluating which makes/models rank well in a specific city, comparing auction
  vehicles, or asking what sports cars / SUVs / trucks would perform best in a given market.
  Also trigger when the user asks about recon costs, days to sell, seller types, rental car
  risk, local market conditions, tariff impacts, warranty value, or how to weight any factor
  in a used car buy decision for any geography. The skill automatically researches and builds
  a city market profile for any city provided, then applies the Elite v4 scoring model with
  city-specific parameters. Fully pre-built profiles exist for Salt Lake City, Houston,
  Dallas, Miami, Denver, Phoenix, Seattle, Atlanta, Las Vegas, and Portland.
---

# Arbitrage Engine — Elite v4 (Universal)

Universal auction vehicle scoring and ranking system. Works for any city.
All market-specific parameters live in city profiles — the scoring engine is universal.

---

## Step 0 — Always identify the target market first

Before scoring anything, confirm the target city. If not specified, ask:
> "Which city or metro area are you buying for? I'll load the local market profile
> to ensure scoring reflects actual demand in that market."

Once city is known:
1. Check if market-profile/[city-slug].md exists in this skill
2. If yes: load it, note profile confidence tier in output header
3. If no: run research/market-research-workflow.md to build profile on the fly
4. Load engine/scoring-core.md alongside the city profile
5. Score using city profile parameters — never apply another city's values

**Profile confidence tiers (always state in output header):**
- Validated: built from live auction + retail data (SLC only) — highest accuracy
- Pre-built: researched and stored in skill — good baseline, refresh for current conditions
- Researched: built from web search this session — directionally correct, validate locally
- Estimated: city not found, using nearest regional proxy — flag uncertainty prominently

---

## Operating Mode

**Client mode — DEFAULT.** Used when sourcing a vehicle for a specific buyer.
- Recon flags functional blockers and FMV detractors only — repair cost is not the metric
- No target gross, no flooring rate, no days-to-sell weighting
- Output centers on drive-out price, local dealer comparison, and client savings
- Drive-out = bid + $1,500 DK service fee + $750 Manheim fee + transport
- Transport from Manheim Nevada: **$0** (standing arrangement — omit from drive-out)
- See scoring-core.md "Client Mode" section for full output spec

**Dealer mode — OPT-IN.** Full resale/flip analysis.
- Triggered when user says: "dealer mode", "flip", "resale", "inventory buy", or "what should we stock"
- Full Elite v4 scoring: GPD, confidence tiers, dual bid output, target gross
- All dealer parameters below apply

---

## Reference file routing

| Task | Load these files |
|---|---|
| Score / rank from CSV or VIN list | engine/scoring-core.md + city profile |
| Analyze condition report PDF | engine/condition-pdf-analysis.md + city profile |
| Seller type / rental risk | engine/seller-analysis.md |
| What sells well in [city]? | city profile + engine/segment-guidance.md |
| Build a new city profile | research/market-research-workflow.md |
| Export to PDF or CSV | engine/output-formats.md |
| Recon cost lookup | engine/scoring-core.md — Recon Cost Table section |

---

## Core ranking formula (Elite v4 — universal, never changes)

  Elite Score = (Est. Gross / Days to Sell) * Confidence Multiplier + City Demand Tiebreaker

Confidence gates:
- Below 50%: auto-PASS
- 50-64%: C-tier
- 65-79%: B-tier
- 80%+: A-tier eligible (also requires GPD >= $120 and gross >= target gross)

Dual bid output — always required:
- Best-case max bid: WIP resolved, low-end recon
- Worst-case max bid: WIP open, high-end recon
- Gap > $1,500: require floor confirmation before bidding above worst-case

---

## Universal rules (apply in every city — never override with city profile)

1. Item-based recon always — never estimate from grade alone
2. WIP risk premium: +$500/item if inspection gap > 30d before auction; +$750 if < 30d
3. Key/fob cost by seller type: Hertz = $350; HMA OEM = $0; Avis/Sixt/Enterprise = $350
4. Dual bid output is mandatory — never output a single max bid number
5. Title "Not Specified" on non-Hertz unit = call floor before bidding
6. Canadian import = -$2,500 retail ceiling, -25 confidence, often auto-PASS
7. Structural condition items = auto-PASS, in-person inspection required
8. Prior paint on any panel = -10 confidence, flag for retail disclosure
9. Accident count > 0 = -15 confidence, mandatory retail disclosure note

---

## Dealer parameters (dealer mode only — user-adjustable)

Target gross:         $3,500
Flooring per day:     $35
Transport:            see transport table in scoring-core.md
Auto-PASS threshold:  50% confidence
A-tier threshold:     80% confidence
C-tier threshold:     65% confidence
