# Seller Analysis — Universal Engine

Seller trust tiers, risk assessment, and buffer rules.
These apply in every market — city profiles do not override seller logic.

---

## Seller tier classification

### Tier 1 — Highest trust (OEM / Institutional)

**Hyundai Motor America / co Car (HMA OEM)**
- Factory direct — from Hyundai corporate fleet to auction
- Always includes remaining warranty and invoice price in Announcements
- 2 keys / 0 fobs standard — $0 key cost
- Watch: inspection-to-auction gap often 40–55 days (sitting lot risk)
- Seller buffer: $0 (clean) / $300 (WIP items present)
- Common units: Tucson, Santa Fe, Santa Cruz, Kona, Sonata N-Line

**Kia Motors Finance / America (OEM direct)**
- Same OEM trust tier as HMA
- Seller buffer: $100
- Watch: confirm AWD on Telluride/Sorento (FWD common on lower trims)

**Mazda Motors of America**
- Factory OEM — very clean, low-mileage demo / fleet
- Seller buffer: $100

**LDS Church Fleet (Element/CPB — Church of Jesus Christ LDS)**
- Institutional maintenance, highest mechanical trust
- Common units: Subaru Outback Premium
- Seller buffer: $75
- Note: "LDS fleet" on Carfax is a positive retail signal in any market

**Toyota / Honda / Lexus dealer CPO trade**
- Seller buffer: $150

---

### Tier 2 — Rental fleet (moderate trust)

**The Hertz Corporation**
- Standing rule: 1 key / 0 fobs → $350 key cost always
- Green Light: when announced, −$150 buffer + post-sale arbitration
- No Green Light: $350 buffer, no post-sale remedy
- Title "Not Specified" is common — call floor to confirm clean
- Seller buffer: $150 (Green Light) / $350 (no Green Light)

**Avis Corporation**
- No Green Light program
- Seller buffer: $350
- Often higher mileage than Hertz equivalents

**Sixt Rent A Car**
- European brand units common — double penalty applies
- Seller buffer: $350
- 1 key / 0 fobs standard — $350 key cost

**Enterprise Vehicle Exchange / Rental**
- Seller buffer: $350

---

### Tier 3 — Finance / lease termination (elevated risk)

**Finance companies (Driveway Finance, credit unions, CULA, Ally)**
- Final-sale, no arbitration, no recourse
- Seller buffer: $500

**Hyundai Motor Finance / Kia Motors Finance (when separate from HMA OEM)**
- Lease termination — different from HMA OEM factory fleet
- Seller buffer: $400

---

### Tier 4 — Independent dealer / remarketing (variable risk)

**Independent dealer**
- Seller buffer: $400–$450
- Require PDF before bidding

**Remarketing companies**
- Seller buffer: $400

**U-Haul Truck and Equipment Sales**
- Commercial fleet — real working use
- Seller buffer: $600

---

## Seller name → tier mapping (Manheim export)

| Seller name (export field) | Tier | Buffer | Key assumption |
|---|---|---|---|
| Hyundai Motor America/co Car | 1 — HMA OEM | $0 / $300 WIP | 2 keys, $0 |
| Kia Motors Finance | 1 — OEM | $100 | Confirm from PDF |
| Mazda Motors Of America Inc | 1 — OEM | $100 | 2 keys likely |
| Rmktg By Element/cpb Church Of Jesus Christ Lds | 1 — LDS Fleet | $75 | 2 keys |
| The Hertz Corporation | 2 — Rental | $150 / $350 | 1 key, $350 |
| Sixt Rent A Car Llc | 2 — Rental | $350 | 1 key, $350 |
| Avis Corporation | 2 — Rental | $350 | 1 key, $350 |
| Enterprise Veh Exchange/rental | 2 — Rental | $350 | 1 key, $350 |
| Driveway Finance Corporation | 3 — Finance | $500 | Unknown, flag |
| Credit Union Leasing Of America | 3 — Finance | $500 | Unknown, flag |
| Hyundai Motor Finance | 3 — HMA Finance | $400 | Unknown, flag |
| Unique Autos Inc | 4 — Dealer | $400 | Unknown, flag |
| Towbin Dodge Ram | 4 — Dealer | $400 | Unknown, flag |
| Meridian Remarketing | 4 — Remarketing | $400 | Unknown, flag |
| Las Vegas Motorcars Llc | 4 — Dealer | $400 | Unknown, flag |
| Chapman Jeep | 4 — Dealer | $400 | Unknown, flag |
| Findlay Motor Company | 4 — Dealer | $400 | Unknown, flag |
| Lexus Of Las Vegas | 4 — Dealer | $400 | Unknown, flag |
| Elite Capital Resource Solutions | 3 — Finance/Fleet | $450 | Unknown, flag |
| Friendly Ford | 4 — Dealer | $400 | Unknown, flag |

---

## Rental car risk — detailed

**The real Hertz risks (not "abuse"):**
1. One key / no fob — $350 hard cost on every unit, every market
2. Cosmetic parking lot wear — exactly what the condition report shows
3. Southwest terrain use (mountain grades, desert heat) — check fluids and brakes
4. Title "Not Specified" — always verify before bidding

**Retail stigma by market:**
- SLC, Denver, Phoenix: ~30–40% of buyers ask about rental history
- Miami: international cash buyers care less about rental history
- Houston, Atlanta: buyers ask but accept with proper disclosure
- Portland, Seattle: eco-conscious buyers may care more — lead with maintenance records

**The Green Light answer to rental stigma:**
"This was a Hertz corporate fleet vehicle, serviced at authorized dealers on OEM
maintenance schedules." Lead with the maintenance record, not the rental use.

**Sixt difference from Hertz:**
- Skews European brand units (GLA, GLE, BMW, Audi)
- No Green Light — full buffer always
- European brand + rental = double confidence penalty
