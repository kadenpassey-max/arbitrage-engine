# Arbitrage Engine — Elite v4

A Claude Code skill for scoring and ranking used cars at Manheim and ADESA auctions. Tells you which cars are worth bidding on, how much to bid, and why — adjusted for the specific city you're buying in.

---

## What it does

- **Scores and ranks vehicles** from Manheim/ADESA CSV exports using the Elite v4 model
- **Calculates dual max bids** — best-case and worst-case — based on MMR, recon, seller type, holding costs, and target gross
- **Analyzes condition report PDFs** to replace grade-estimated recon with real line-item costs
- **Applies city-specific demand data** — days to sell, AWD premiums, brand trust, color penalties, and segment demand vary by market
- **Classifies sellers** — OEM fleet, rental, finance, and independent dealer tiers each carry different trust levels and cost buffers
- **Flags risk automatically** — Canadian imports, structural damage, WIP items, title issues, and accident history all trigger automatic rules

---

## How to use it

Open Claude Code and type:

```
/arbitrage-engine
```

It will ask for your target city first, then you can:

- Paste or attach a **Manheim/ADESA CSV export** to get a full ranked buy list
- Paste a **single listing** (year, make, model, trim, mileage, MMR, grade, seller) to score one car
- Attach a **condition report PDF** to get a precise item-based recon estimate and updated score
- Ask **"what sells well in [city]?"** to get segment and make/model guidance for that market

---

## Scoring tiers

| Tier | Confidence | What to do |
|---|---|---|
| **A** | 80%+ | Pull condition report PDF and bid |
| **B** | 65–79% | Watch, bid conservatively |
| **C** | 50–64% | Low priority |
| **Pass** | Below 50% | Skip |

---

## Bid output

Every score produces two numbers:

- **Best-case max bid** — WIP items assumed resolved, low-end recon
- **Worst-case max bid** — WIP risk premiums applied, high-end recon

If the gap between the two exceeds $1,500, the engine flags it and recommends calling the Manheim floor before bidding above worst-case.

Default parameters: $3,500 target gross · $300 transport · $35/day flooring

---

## Pre-built city profiles

Full market profiles are included for:

Atlanta · Dallas · Denver · Houston · Las Vegas · Miami · Phoenix · Portland · Salt Lake City · Seattle

For any other city, the engine researches and builds a profile on the fly using current market data.

---

## File structure

```
SKILL.md                          — Skill definition and routing logic
engine/
  scoring-core.md                 — Universal scoring formula, recon table, bid math
  condition-pdf-analysis.md       — How to extract and apply condition report data
  seller-analysis.md              — Seller tier classification and buffer rules
  output-formats.md               — Ranked list and export formatting
market-profile/
  [city].md                       — City-specific demand parameters
  PROFILE-TEMPLATE.md             — Template for building new city profiles
research/
  market-research-workflow.md     — How to build a profile for an unlisted city
```

---

## Installation

Copy the skill folder into your Claude Code skills directory:

**Mac/Linux:**
```
~/.claude/skills/arbitrage-engine/
```

**Windows:**
```
C:\Users\[username]\.claude\skills\arbitrage-engine\
```

Then start a new Claude Code session — the skill will be available as `/arbitrage-engine`.
