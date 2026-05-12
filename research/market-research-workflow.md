# Market Research Workflow

How to build a city profile from scratch using web research.
Run this workflow when a city profile does not exist in `market-profile/`.

---

## When to run this workflow

- User specifies a city not in the pre-built profile list
- User asks "what works well in [city]?" for an unproofiled market
- User uploads vehicles targeting a new market

**Time required:** 5–8 web searches, approximately 2–4 minutes.

**Output:** A completed city profile saved as `market-profile/[city-slug].md`
with profile tier: RESEARCHED.

---

## Research sequence

Run these searches in order. Extract the specific data points listed after each.

### Search 1: Segment velocity
```
Query: "fastest selling used cars [city] [current year] days on market"
Alternative: "used car market [city state] demand segments [year]"
```
Extract:
- Which segments sell fastest (SUV, truck, sedan, sports car, minivan)
- Any city-specific DTS data (days on market by segment)
- Market velocity vs. national average (faster / slower / similar)

### Search 2: AWD and climate demand
```
Query: "[city] AWD all-wheel drive demand winter weather used cars"
Alternative: "[city state] snowfall average inches winter driving"
```
Extract:
- Does the city have real winters? (snowfall > 10" annual = significant AWD demand)
- Local AWD premium estimate (0% = no winter, 5% = heavy winter market)
- Which months are slow (if seasonal market)

### Search 3: Demographics and buyer profile
```
Query: "[city] demographics median household income population age family size 2024 2025"
```
Extract:
- Median household income (determines price sensitivity and luxury market depth)
- Median age (younger = value-oriented, sports cars; older = luxury, trucks)
- Average household size (larger = minivan/3-row demand)
- Notable community characteristics (military, college, tech, outdoor lifestyle)

### Search 4: Local market leaders / top-selling vehicles
```
Query: "[city] best selling used cars [year] dealer inventory popular models"
Alternative: site:cargurus.com [city] most searched used cars
```
Extract:
- Top 5 most searched / listed models in this market
- Price ranges that move fastest
- Any locally popular model not common nationally (e.g., trucks in Houston,
  convertibles in Miami, AWD crossovers in Denver)

### Search 5: Financing landscape
```
Query: "[city] used car financing rates credit unions banks [year]"
Alternative: "[city state] auto loan rates average 2025"
```
Extract:
- Dominant local lenders (credit unions vs. banks vs. captive finance)
- Average used car loan rate in market
- Whether European brand financing is notably harder or easier here vs. national

### Search 6: Color and seasonal demand (optional but useful)
```
Query: "[city] most popular car colors used car market OR [city] used car buying season"
```
Extract:
- Regional color preferences (sunbelt markets favor white/silver; Northeast accepts dark)
- Seasonal demand patterns (snowbird markets have spring/fall spikes)

### Search 7: Validate with live inventory
```
Web fetch: https://www.cargurus.com/Cars/l-Used-Cars-[city]-d[id]
OR search: "used car inventory [city] CarGurus Edmunds most popular"
```
Extract:
- Actual live inventory to validate segment assumptions
- Price distribution (what's actually selling and at what price points)
- Supply gaps (segments with few listings = faster turns when you have one)

---

## Profile construction

After completing searches, fill in the PROFILE-TEMPLATE.md structure:

**Confidence calibration by data quality:**
- Found specific DTS data for this city → high confidence on that metric
- Only found national data → use national default, flag as estimated
- Contradictory data sources → note range, use conservative estimate
- No data found for a metric → fall back to nearest regional equivalent

**Regional fallback hierarchy:**
If data is sparse for a specific city, use the nearest equivalent:
- Mountain West cities → use SLC profile as base, adjust for climate delta
- Gulf Coast cities → use Houston profile as base
- Pacific Northwest → use Seattle profile as base
- Southeast → use Atlanta profile as base
- Southwest desert → use Phoenix profile as base
- South Florida → use Miami profile as base

---

## Profile validation check

Before saving the profile, verify:
- [ ] AWD premium is appropriate for the climate (0–5%)
- [ ] DTS numbers are internally consistent (fastest segment < 20d only if strong justification)
- [ ] Demand bonuses sum to no more than 15% total across all bonuses
- [ ] At least one "avoid" segment identified
- [ ] Profile tier marked as RESEARCHED (not VALIDATED)
- [ ] Research date recorded

---

## Saving the profile

Save as: `market-profile/[city-slug].md`

City slug format: lowercase, hyphens for spaces, no state abbreviation unless
ambiguous (e.g., `portland-or.md` vs `portland-me.md`).

Note at top of file:
```
Profile tier: RESEARCHED
Research date: [date]
Data sources: [list search queries that returned useful data]
Confidence: [HIGH / MEDIUM / LOW] — explain briefly
```

---

## Ongoing refinement

A RESEARCHED profile should be treated as a starting hypothesis, not ground truth.
As the user runs actual auctions and retail deals in that market:
- Note which vehicles turned faster or slower than the profile predicted
- Note which demand bonuses were confirmed or contradicted by retail outcomes
- After 10–20 real deals, the profile can be upgraded from RESEARCHED to CALIBRATED

The SLC profile reached VALIDATED status after tracking 69 real auction vehicles,
9 PDF condition reports, and live retail outcome feedback. That's the gold standard.
