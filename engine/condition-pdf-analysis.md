# Condition Report PDF Analysis

How to read Manheim detail page PDFs and integrate findings into v4 scoring.

---

## What to extract from every PDF

### Page 1 — Summary
- VIN (confirm matches CSV/list)
- Mileage, drivetrain, engine, transmission, body
- Adj MMR range (upper and lower — use midpoint for scoring but note range)
- Seller name
- AutoGrade score and designation (Clean / Extra Clean)
- Condition item count (exterior / interior / structure / other)
- Key count / fob count
- Search count and view count (demand signal)
- Green Light announced? (check Announcements section)

### Page 2 — Vehicle History
- Owner count (1 is ideal)
- Accident count (0 is ideal — any accident is a confidence penalty and retail disclosure)
- Title / problems flag
- ODO flag
- Note: CARFAX link is present but you cannot access it — flag for dealer to pull

### Page 3 — Condition Details
- All condition items with description, condition severity, and repair status
  - Status options: blank (no action), Repair Completed (green), Work in Progress (bold)
  - WIP status = apply WIP risk premium in recon
- Condition diagram (exterior panel map — yellow = flagged panels)
- Title status (Title Present = clean; Not Specified = flag; Salvage/Rebuilt = PASS)
- Title state (flag Canadian provinces — apply Canadian import penalties)
- Inspection date (calculate gap to auction date)

### Page 4 — Overview + Tires
- Exterior color code (map to color DTS adjustment)
- Interior color
- Seating capacity (confirms 3-row if 7 or 8 seats)
- Base MSRP (useful for retail ceiling anchor with buyers)
- Tire brand, tread depth (in 32nds), size for all four corners + spare
  - 10/32: new — no spend
  - 8–9/32: excellent — no spend
  - 6–7/32: good — no spend, but note
  - 5/32: borderline — disclose to retail buyer, budget $150 for negotiation
  - 4/32: borderline — replace recommended, add $150/tire
  - 3/32 or less: must replace, add $250/tire
  - Spare: full-size spare at 8/32+ = +$100 retail credit; compact spare = neutral

### Pages 5–7 — Equipment & Options
Key items that affect retail ceiling:
- Uconnect 5 with 12.3" display (Wrangler) → +$400 retail adj vs base screen
- Bose / AKG / premium audio → +$300 retail adj
- Super Cruise / advanced driver assist → +$500 flat
- Heated + ventilated seats → +$200 retail adj
- Head-Up Display → +$200 retail adj
- Panoramic sunroof (dual row) → +$300 retail adj
- Hard top vs soft top (Wrangler) → hard top +$400 vs soft top equivalent
- 10/100 warranty remaining → confirmed from Announcements field
- Performance package (Z66, Z51, etc.) → note and factor into retail ceiling

### Page 8 — Seller Information
- Confirm seller name matches CSV
- Note if seller has "View Seller's Other Vehicles" — useful for identifying
  additional units from same source

---

## Announcements field — critical data to parse

The Announcements block contains structured seller data. Extract:

| Announcement text | Action |
|---|---|
| GREEN LIGHT | Apply $150 credit to seller buffer. Flag as arbitration-protected. |
| COMP;SR;LTHR;AWD | Confirms: sunroof, leather, AWD — no need to verify in equipment list |
| REMAINING 10/100 WARRANTY | Apply 6% HMA warranty bonus. Note in output. |
| REMAINING 5/60 WARRANTY | Apply 3.5% HMA warranty bonus. |
| USED VEH;NO INCENTIVES/REBATES | Standard HMA OEM language — no adjustment |
| INVOICE $XXXXX | Record as seller cost basis. If MMR is within $1,500 of invoice, floor will be firm. |
| SUPER CRUISE | Add $500 flat to retail ceiling. Note in signals. |
| PERFORMANCE PKG / Z66 / Z51 | Note package, verify equipment, factor into retail ceiling. |
| CANADIAN IMPORT | Apply Canadian penalty: −$2,500 retail ceiling, −25 confidence. Often auto-PASS. |
| Title State = Canadian province (BC, ON, AB, etc.) | Same as Canadian import flag. |

---

## VIN verification workflow

When a PDF is provided for a vehicle already in the ranked list:

1. Extract VIN from PDF header (format: 17 characters, appears in first paragraph)
2. Compare to VIN in ranked list
3. If mismatch: update the ranked list with the confirmed PDF VIN
4. Note previous VIN was estimated if it was not from CSV export
5. Decode key VIN positions:
   - Position 4: model line (P = Wrangler JL, H = Wrangler JK — different generations)
   - Position 7: trim (E = Sahara, N = Sport/Sport S)
   - Position 10: model year (S = 2025, T = 2026, R = 2024)

---

## How PDF data changes the score

After extracting all PDF data, recalculate:

1. **Recon**: Replace grade-estimated recon with item-based recon from condition report
2. **Confidence**: Adjust for actual grade + items + WIP status + title status
3. **Days to sell**: Apply color adjustment from confirmed exterior color code
4. **Retail ceiling**: Apply equipment bonuses confirmed from options list
5. **Seller buffer**: Confirm Green Light, adjust buffer accordingly
6. **Key cost**: Confirm key/fob count, apply standing rule
7. **Demand signal**: Apply search/view adjustment from confirmed data
8. **WIP flag**: If any WIP items, calculate both best-case and worst-case bid
9. **Re-rank**: Update position in ranked list, note movement (↑ / ↓ / =)

---

## Common PDF patterns by seller type

**Hertz Corporation:**
- Almost always: 1 key / 0 fobs — $350 recon standing cost
- Often: Green Light announced
- Often: Title "Not Specified" — call floor to confirm
- Typical condition: 1–4 exterior items, clean interior, serviceable tires
- Watch: rental units driven hard on mountains and canyons in Southwest US

**Hyundai Motor America (HMA OEM):**
- Almost always: 2 keys / 0 fobs — $0 key cost
- Announcements always include remaining warranty and invoice price
- Watch: some units have WIP items despite high grades — read condition report carefully
- Watch: TPMS lights and battery issues common on fleet units that sat in storage
- Inspection-to-auction gap often 40–55 days — flag for sitting lot risk

**LDS Church Fleet (Element/CPB):**
- Institutional maintenance records — highest trust for mechanical condition
- Often very clean interiors (fleet policy — no food, no smoking)
- Outback Premium units common — AWD, solid retail profile

**Sixt / Avis / Enterprise:**
- Similar to Hertz — 1 key, no fob assumed
- Sixt units often higher mileage than Hertz equivalents (longer rental cycles)
- No Green Light — full seller buffer applies
- European brand units from Sixt carry double penalty (Euro brand + rental risk)

---

## Red flags that trigger automatic review

These conditions require escalation before bidding — do not bid blind:

- Title state is a Canadian province
- Title "Not Specified" on a non-Hertz unit (Hertz is expected; others are not)
- Accident count > 0 on AutoCheck / CARFAX summary
- Any structural condition items (always auto-PASS unless inspected in person)
- Prior paint on any panel (indicates prior body work — potential hidden damage)
- Misaligned bumper cover (may indicate hidden impact absorption damage)
- WIP items on a unit with < 30-day inspection gap (repair quality unverifiable)
- Recon estimate exceeds $3,000 (confidence auto-drops below likely bid threshold)
