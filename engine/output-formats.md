# Output Formats

How to structure results for different output requests.

---

## Standard ranked list output

When ranking multiple vehicles, always output:

**Summary header:**
- Total vehicles scored
- A/B/C/PASS breakdown
- Average recon delta (if PDFs reviewed)
- Any notable finds (new vehicles discovered, VIN corrections, etc.)

**Per-vehicle table columns:**
Rank | VIN | Vehicle | Miles | Grade | MMR | Est Gross | $/day | Max Bid (best) | Max Bid (worst) | Conf% | Tier | Signals

**Signal flags (use inline):**
- Green: HMA OEM, Green Light, zero recon, Extra Clean 5.0, warranty, AWD, fast color
- Amber: WIP items, borderline tires, low-demand signal, title not specified, polarizing color
- Red: TPMS on, AC issue, mandatory tire replace, Canadian import, no PDF reviewed

---

## Single vehicle analysis output

When analyzing one vehicle (from VIN, CSV row, or PDF), always output:

1. **Vehicle identification** — year, make, model, trim, VIN, miles, grade, seller
2. **Adj MMR range** from PDF or estimated
3. **Recon breakdown** — line-item table (v4 item-based), total
4. **Signal flags** — positive, warning, risk
5. **Watch items** — anything requiring floor rep call or in-person inspection
6. **Bid math** — est retail, est gross, gross/day, confidence
7. **Dual bid output** — best-case and worst-case max bid, gap, gap color signal
8. **v4 logic** — 3–5 sentence explanation of why the vehicle scored where it did
9. **Floor rep questions** — what to ask before the lane opens (if WIP or title issues)

---

## PDF export (when requested)

Use reportlab with the following structure:

**Cover:**
- "Arbitrage Engine — Elite Model v4"
- Subtitle with auction name and date
- Summary metrics (vehicles scored, PDFs confirmed, avg recon delta, positions changed)

**Per vehicle card (one card = one page if possible):**
- Rank badge (colored by tier: green=A, blue=B, gray=C)
- Vehicle name, VIN, miles, grade, MMR, days, PDF status
- Elite score + confidence
- 4-metric grid: Adj MMR, Est Gross, Recon, Gross/day
- Recon breakdown table (item-based)
- Signal flags (color-coded pills)
- Watch items (amber block)
- Dual bid boxes: best-case (green) and worst-case (amber)
- Logic paragraph

**Appendix pages:**
- Model v4 parameters table
- Recon cost reference table

Use the PDF generation code pattern from this session's top10.py as the template.
Colors: GREEN_DARK=#27500A, BLUE_DARK=#0C447C, AMBER_DARK=#633806, RED_DARK=#791F1F

---

## CSV export (when requested)

Column order:
rank, vin, year, make, model, trim, segment, miles, grade, mmr, markup_pct, recon_best,
recon_worst, est_retail, est_gross, days_to_sell, gross_per_day, max_bid_best,
max_bid_worst, bid_gap, confidence, tier, elite_score, seller, seller_tier, drivetrain,
color, tires, keys, warranty, pdf_confirmed, wip_items, searches, views, signals

Flag estimated VINs with "ESTIMATED:" prefix so they're easily filterable.

---

## Inline ranked widget (when user is viewing in Claude.ai)

Use the interactive HTML widget pattern with:
- Tap-to-expand rows
- Rank number colored by tier (green=top 3, blue=mid, gray=bottom)
- VIN in monospace
- Elite score displayed prominently
- Dual bid shown in green/amber boxes on expand
- "Live market check ↗" and "Floor rep questions ↗" sendPrompt buttons

Reference the widget code from the elite_v4_top10_clean widget in this session.

---

## Floor rep question templates

Generate these when a vehicle has WIP items or title concerns:

**WIP repair:**
"Work order [WORK_ORDER_NUMBER] from [INSPECTION_DATE] — can you confirm the [REPAIR_DESCRIPTION]
repair has been completed and the vehicle is in the condition described in the closed work order?"

**AC / gas missing WIP:**
"Work order [NUMBER] shows 'Gas Missing / Work in Progress' — can you confirm whether the
AC system has been recharged and is functioning, or whether there was a component failure?"

**Title not specified (Hertz):**
"The title shows 'Not Specified' — can you confirm this is a clean title and there are no
outstanding liens, salvage brands, or title issues before I bid?"

**Tire condition borderline:**
"The condition report shows [CORNER] tire at [DEPTH]/32. Can you walk the car and confirm
whether that tire has been replaced since the inspection date of [DATE]?"
