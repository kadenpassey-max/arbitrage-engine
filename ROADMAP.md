# Arbitrage Engine — Feature Roadmap

Ideas developed May 2026 for expanding the engine beyond general inventory 
arbitrage into a full client-service brokerage workflow tool.

---

## Context

The current engine is optimized for: buy low at auction → sell at retail → 
maximize gross/day. The expanded use case is a **buyer's agent / broker model** 
where vehicles are sourced for specific clients, not just for open inventory. 
This requires a second mode that scores vehicles against client needs, not just 
market demand.

---

## Feature Ideas

### 1. Client Order Matching (highest priority)

**Problem:** After scoring a Manheim CSV, the user manually matches vehicles to 
active client orders. This is a second mental pass that takes time and relies on 
memory.

**Solution:** User uploads two files — the Manheim CSV and a client order 
spreadsheet. The engine cross-references automatically and outputs which vehicles 
match which clients, with bid parameters per client.

**Output example:**
> Unit #4 — 2024 RAV4 XLE AWD, 38k mi, Grade 4.4 — matches Sarah Kim 
> (spec match, within budget, urgent 10-day timeline). Recommend bid up to $26,200.

**Client spreadsheet column format:**
Client Name | Priority (Hot/Warm/Cool) | Budget OTD | Target Year Range | 
Make/Model | Trim Floor | Drivetrain | Max Miles | Timeline (days) | 
Rental OK (Y/N) | Financing or Cash | Color Notes | Special Notes | 
Status (Active/On Hold/Found)

---

### 2. Hunt Card Generator

**Problem:** When a new client comes in with a vehicle in mind, the parameters 
live in the rep's head. There is no standardized document a rep can carry and 
act on independently.

**Solution:** User inputs a brief client description (conversational or from the 
spreadsheet). Engine generates a one-page hunt card per client containing:

- Buying parameters: target MMR range, max bid (best/worst case), grade floor, 
  mileage ceiling
- Seller preference order (ranked 1–5 with buffers)
- Seller types to avoid for this specific client
- Model-specific intelligence: trim nuances, known WIP items, engine 
  generation considerations, cab/bed preferences, drivetrain verification notes
- Color priority table with DTS impact
- Manheim search parameters (copy-paste ready)
- Talking points for client updates

**Key value:** Standardizes what "good" means across the team. A newer rep 
can execute a search competently without needing the manager to review every 
vehicle decision.

---

### 3. Master Hunt List View

**Problem:** With multiple active client orders, there is no fast way to see 
the full picture — what are we hunting, who is most urgent, what's the MMR 
target for each?

**Solution:** Drop in the client spreadsheet and ask "what are we hunting right 
now." Engine outputs a color-coded priority board:

> ACTIVE ORDERS — 6 clients
> 🔴 Marcus Webb — 2022–23 Tacoma TRD Off-Road, 4WD, $27–30k MMR, 14 days
> 🔴 Sarah Kim — 2023–24 RAV4 AWD, $22–25k MMR, 10 days (urgent)
> 🟡 Jason Torres — 2021–23 Wrangler Sahara, $26–30k MMR, 30 days
> ...

**Use case:** Start of day briefing. Also useful when at auction and deciding 
where to focus attention across multiple lanes.

---

### 4. Multi-Market Pricing for Client Deliveries

**Problem:** If a client is in Phoenix but the vehicle is sourced at Manheim 
Utah, the current engine prices against SLC retail demand — not the client's 
actual market.

**Solution:** Hunt card and bid calculation mode that accepts a client's home 
city and reprices the vehicle against that city's market profile, not SLC. 
Transport cost updates accordingly.

---

### 5. Follow-Up Communication Templates

**Problem:** Follow-up texts and call openers are written fresh each time, 
adding mental overhead across dozens of leads and clients.

**Solution:** Template library tuned to funnel stage. User inputs brief context 
("lead went cold after initial call, interested in a Tacoma, mentioned budget 
was tight") and gets a ready-to-send text or call opener.

**Funnel stages to cover:**
- New inbound lead (first contact)
- Lead who hasn't responded in 3–5 days
- Lead who hasn't responded in 10+ days
- Active client check-in (vehicle still being hunted)
- Client whose vehicle just arrived — handoff message
- Post-purchase referral ask

---

### 6. Sales Objection Handler

**Problem:** Objections are handled inconsistently across reps. New reps 
especially struggle with price, mileage, and rental history objections.

**Solution:** Input an objection and get a response script calibrated to the 
vehicle type and buyer profile. Also supports simulation mode — engine plays 
a difficult buyer and scores the rep's responses.

**Common objections to cover:**
- "CarMax is showing it cheaper"
- "Why does it have 40k miles"
- "I don't want a rental car"
- "I can get financing cheaper at my bank"
- "I want to think about it"
- "I found one on Facebook Marketplace for less"

---

### 7. Training Resource Generator

**Problem:** Building training materials for new reps takes significant time — 
outlines, scripts, and quiz questions need to be written from scratch.

**Solution:** Input a topic and get a structured training outline, script, and 
quiz questions formatted for a slide deck or PDF.

**Topics to support:**
- How to read a Manheim condition report
- How to explain MMR to a client
- How to handle rental history objections
- How to walk a client through the sourcing timeline
- Seller type tiers and what they mean for recon risk
- How to use the hunt card system

---

### 8. Deal Memo / Client Delivery Summary

**Problem:** Once a vehicle is purchased for a client, the handoff is verbal. 
Clients ask repeat questions and the process feels informal.

**Solution:** After a vehicle is sourced, generate a one-page client-facing 
deal memo containing:
- Vehicle specs and condition summary
- What was paid vs. what the client owes
- Expected delivery timeline
- Key disclosure items (rental history, mileage, condition grade)
- Next steps

Makes the handoff professional and reduces back-and-forth.

---

## Implementation Notes

- All new modes should activate based on what the user uploads and how they 
  phrase the request — no need to specify a mode name explicitly
- Hunt card intake should work conversationally ("client wants a clean RAV4 
  AWD, budget around $28k") or from the structured spreadsheet
- Client spreadsheet is the persistent memory layer for client data — not 
  Claude's memory system, which is reserved for dealer parameters and workflow 
  preferences
- Dealer defaults (target gross $3,500, flooring $35/day, transport $300, 
  primary market SLC) should pre-load in every session without prompting

---

## Session Startup Workflow (target state)

1. User pastes or uploads client order spreadsheet
2. Engine generates master hunt list and refreshes any stale hunt cards
3. User uploads Manheim CSV
4. Engine cross-references CSV against active orders and outputs client-matched 
   vehicle recommendations alongside general A-tier arbitrage plays
5. User bids with dual max bid output per vehicle, per client
