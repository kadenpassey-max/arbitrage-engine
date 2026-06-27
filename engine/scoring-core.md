# Scoring Core — Universal Engine

Market-agnostic scoring logic. All city-specific values (DTS, demand bonuses,
AWD premium) come from the loaded city profile, not from this file.

---

## Client Mode — Default Scoring & Output

Client mode is the default. Use it when sourcing a vehicle for a specific buyer.
Switch to Dealer Mode only when explicitly requested.

### Drive-out price formula

```
Drive-out = Bid + $1,500 (DealerKey service fee) + $750 (Manheim fee) + Transport
```

Transport from Manheim Nevada = $0 (standing arrangement).
All other auction locations: use transport table below.

### Local dealer comparison (always include in client mode output)

```
Est. UT retail     = MMR × brand markup% (from city profile)
UT dealer drive-out = Est. UT retail + $599 (UT doc fee)
Client savings      = UT dealer drive-out − DealerKey drive-out
```

A positive savings number is the client arbitrage — what they keep in their pocket
vs. buying the equivalent vehicle off a Utah lot.

### Value spread

```
Value spread = MMR − DealerKey drive-out
```

Positive = client is buying below wholesale market value. Negative = paying over MMR;
only appropriate when condition, mileage, or feature set justifies it.

| Spread | Signal |
|---|---|
| > $2,000 under MMR | Strong buy — flag as high value |
| $0–$2,000 under MMR | Fair — on-market |
| Negative (over MMR) | Flag — explain why it's still justified, or pass |

### Recon in client mode — three tiers

Do NOT calculate repair cost as the primary metric. Instead classify each item:

**Functional blocker** — always flag, must disclose, affects whether client can use the car:
- Battery degradation / failure
- AC system failure (not just recharge — compressor/component failure)
- Transmission issues
- Engine issues
- Structural damage
- Safety system failures (airbag, ABS)
- Any item that would require repair before the car is drivable or safe

**FMV detractor** — note with estimated market value impact, not repair cost:
- Significant body panel damage, major dents, missing trim
- Prior paint (disclosed, but affects resale if client ever sells)
- Heavy interior wear or damage
- Significant wheel damage
Use judgment: "This damage would likely reduce the car's market value by $X–$Y
compared to a clean example."

**Cosmetic note** — document but assign no financial weight:
- Rock chips, light scratches, normal wear
- Minor interior scuffs
- Stone chips on leading edges

### Client mode output format

For each vehicle, output in this order:
1. Vehicle ID (year / make / model / trim / miles / grade / VIN)
2. MMR
3. DealerKey drive-out price (bid + $2,250 + transport)
4. UT dealer drive-out estimate + **client savings line**
5. Value spread (MMR vs. drive-out)
6. Functional blockers (if any) — prominent, before anything else
7. FMV detractors (if any) — with estimated market value impact
8. Cosmetic notes (brief)
9. Confidence signal (grade, seller tier, miles)
10. Recommended max bid

---

## Transport Cost Table (SLC as destination)

Applies in both modes. Nevada column reflects standing client arrangement.

| Auction location | Client mode | Dealer mode |
|---|---|---|
| Manheim Utah | $0–$100 | $0–$100 |
| Manheim Nevada (Henderson) | **$0** | $250–$350 |
| Manheim Phoenix | $350–$450 | $350–$450 |
| Manheim Denver | $350–$450 | $350–$450 |
| Manheim Seattle | $350–$450 | $350–$450 |
| Manheim California (inland — El Monte, Fontana) | $400–$500 | $400–$500 |
| Manheim Riverside | $450–$550 | $450–$550 |
| Manheim Southern California (Studio City, etc.) | $450–$550 | $450–$550 |
| Manheim Oceanside | $450–$550 | $450–$550 |
| Manheim San Francisco Bay | $500–$650 | $500–$650 |
| Manheim Dallas / Dallas-Fort Worth | $600–$800 | $600–$800 |
| Manheim Houston / Texas | $700–$900 | $700–$900 |
| Manheim Atlanta | $900–$1,100 | $900–$1,100 |

When transport is a range, use midpoint for drive-out calculation and note the range.
For dealer mode, use midpoint in bid math; flag high-end as worst-case scenario.

---

---

## Recon cost table (universal — confirmed from live auction data)

| Condition item | Cost |
|---|---|
| Exterior scratch light | $150 |
| Exterior scratch heavy | $400 |
| Panel dent single — PDR | $250 |
| Panel dent multiple — PDR | $600 |
| Panel dent multiple + paint damage | $900 |
| Windshield pitted (replace + ADAS recal) | $500 |
| Windshield chip | $200 |
| Tail lamp scratch heavy | $450 |
| TPMS warning light (sensor fix) | $250 |
| Tire worn single — mandatory replace | $250 |
| Tire 4/32 borderline (single) | $150 |
| Dash cluster lens heavy scratches | $650 |
| Bumper cover chipped | $200 |
| Bumper cover misaligned (clip failure) | $350 |
| Bumper cover misaligned (impact suspected) | $800 |
| Radio inop — repair completed | $0 |
| Battery inop — repair completed | $0 |
| Maintenance light on (unknown cause) | $150 |
| Prior repair disclosed | $250 risk buffer |
| WIP risk premium per item (gap > 30d) | $500 |
| WIP risk premium per item (gap < 30d) | $750 |
| Key replacement — proximity system | $350 |
| Key replacement — standard | $200 |
| Full-size spare 9/32+ (credit) | −$100 |

**Rule:** Start recon at $0. Add line items from condition report.
Do not estimate recon from grade alone — grade is a confidence modifier only.

**Grade as confidence modifier:**
- 5.0 Extra Clean, zero items: +15 conf
- 4.8–4.9: +5 conf
- 4.3–4.7: neutral
- 4.0–4.2: −10 conf
- Below 4.0: −20 conf

---

## Brand markup (base rates — adjust per city profile if noted)

| Classification | Base markup |
|---|---|
| Domestic (Jeep, Ford, Chevy, GMC, RAM, Dodge, Buick, Cadillac, Chrysler) | 18% |
| HMA (Hyundai, Genesis, Kia) | 18% |
| Subaru | 18% |
| Mazda | 18% |
| Honda / Toyota / Lexus | 15% |
| Nissan / Mitsubishi / Infiniti | 15% |
| European (BMW, Mercedes, Audi, Volvo, VW, Land Rover, Porsche, Alfa Romeo) | 12% |

City profiles may adjust these up or down based on local brand trust and
financing availability. Always check the city profile's "Brand adjustments" section.

---

## Seller buffer (universal)

| Seller type | Buffer |
|---|---|
| HMA OEM — zero condition items | $0 |
| HMA OEM — with WIP items | $300 |
| LDS / institutional fleet | $75 |
| Hertz — Green Light announced | $150 |
| Hertz — no Green Light | $350 |
| Avis / Sixt / Enterprise / other rental | $350 |
| OEM factory (Mazda, Honda, Toyota, Ford Motor direct) | $100 |
| Lease finance company | $500 |
| Independent dealer | $400 |
| Unknown / not specified | $450 |

---

## Confidence score calculation (universal base)

Start at 75. Apply:

| Factor | Adj |
|---|---|
| Grade 4.8+ | +15 |
| Grade 4.5–4.7 | +5 |
| Grade 4.3–4.4 | 0 |
| Grade 4.0–4.2 | −10 |
| Grade below 4.0 | −25 |
| Miles < 5,000 | +10 |
| Miles 5,000–15,000 | +5 |
| Miles 15,001–35,000 | 0 |
| Miles 35,001–50,000 | −8 |
| Miles > 50,000 | −15 |
| OEM seller (HMA, Mazda, Honda factory) | +12 |
| Institutional fleet (LDS, corporate) | +10 |
| Green Light announced | +5 |
| Finance / lease seller | −12 |
| WIP item(s) on condition report | −10 per item |
| Recon > $2,500 | −18 |
| Recon $1,500–$2,500 | −10 |
| Canadian import | −25 |
| European brand (financing friction) | −8 |
| Searches < 300 / views < 3 | −5 |
| Searches > 1,500 / views > 15 | +5 |
| Title "Not Specified" (non-Hertz) | −10 |
| Title "Not Specified" (Hertz) | −5 |
| Accident on AutoCheck / CARFAX | −20 |
| Inspection gap > 45 days | −3 |

Cap: 97. Floor: 10.

---

## Dealer Mode — Bid Math Formula

```
Est. Retail = MMR × (1 + Markup%) 
            + [City AWD bonus if AWD/4WD]
            + [City segment demand bonus]
            + [Feature bonuses from city profile]
            + [Warranty bonus if applicable]
            − [Canadian import penalty if applicable]
            − [Color penalty if applicable per city profile]

Holding Cost = Days to Sell (city profile) × Flooring Rate ($35/day default)

Best-Case Recon = Line items, low-end estimates, WIP assumed resolved
Worst-Case Recon = Line items, high-end estimates, WIP risk premiums included

Best-Case Max Bid = Est. Retail − Best Recon − Transport ($300) 
                  − Holding − Seller Buffer − Target Gross ($3,500)

Worst-Case Max Bid = Est. Retail − Worst Recon − Transport 
                   − Holding − Seller Buffer − Target Gross
```

---

## CSV processing workflow

When given a Manheim / ADESA export CSV:

1. Confirm city profile is loaded before proceeding
2. Parse: VIN, Year, Make, Model, Trim, Miles, MMR, Grade, Seller Name, Drivetrain
3. Classify each vehicle: brand type, segment, seller type, drivetrain
4. Apply city-profile DTS, demand bonuses, markup adjustments
5. Apply standing key cost by seller type (universal rule)
6. Estimate recon from grade tier (no PDF) — note as estimated
7. Calculate confidence from grade + miles + seller + drivetrain
8. Apply search/view signal if available
9. Calculate gross, GPD, max bid (best and worst)
10. Rank by Elite Score = (GPD × Conf Multiplier) + Demand Tiebreaker
11. Output ranked table with tier assignments
12. Flag top A-tier units for PDF review before bidding
13. Note: "Recon estimated from grade — pull PDFs for any unit you intend to bid"

**Priority PDF pull order:** A-tier units by GPD descending.

---

## Inspection date lag

If inspection date > 45 days before auction:
- Add note: "Sitting lot risk — verify battery, TPMS, and tires on arrival"
- −3 confidence
- Common on HMA OEM units with long pipeline

---

## Run position signal

High run numbers (last 25% of lane) often see softer competition as bidder capital
depletes. Note when advising on bidding aggressiveness vs. conservatism.
