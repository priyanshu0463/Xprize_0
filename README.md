# Compliance Co-Pilot

A regulatory threshold-detection and plain-language compliance co-pilot for India's micro-entrepreneurs

---

## 1. The Problem

India added roughly 6.3 crore MSMEs and an unknown multiple of unregistered micro-businesses over the last decade, and almost none of them have a Chartered Accountant on retainer. Compliance in India is not one form — it is a moving target made of overlapping central and state thresholds (GST, Udyam, Shops & Establishment, Professional Tax, ESI, PF, FSSAI) that change as a business grows, and nobody tells the owner when they have crossed a line until a penalty notice arrives.

### 1.1 Why existing platforms don't fix this

- **Siloed by design.** ClearTax, Vakilsearch, IndiaFilings and similar platforms are walled gardens — no data exchange with each other or with India's own Digital Public Infrastructure (DEPA, Account Aggregators, DigiLocker).
- **Upsell-first economics.** Headline pricing is a loss-leader for mandatory annual retainers, often pushed before the original registration is even finished.
- **Reactive, not predictive.** Every platform waits for the owner to come back and ask. None of them watch the business's numbers and proactively flag the moment a legal threshold is about to be crossed.
- **Repetitive documentation.** The same PAN, Aadhaar, and address proof gets re-uploaded at every registry because there is no shared, consented identity layer underneath these platforms.
- **Built for accountants, not owners.** Dense, English-first, legal-register UX excludes exactly the vernacular, voice-first micro-entrepreneur the system is supposed to serve.

> **The actual gap:** Nobody owns the moment of threshold-crossing — the instant a business's revenue, headcount, or activity quietly makes a filing mandatory — and explains it back to the owner in their own language, in one next action, instead of a 40-page checklist.

---

## 2. The Idea

A compliance co-pilot that watches a micro-business's self-reported (and later, consented bank/GST) data against a maintained rules engine of Indian regulatory thresholds, and turns every threshold event into one plain-language, voice-first next action — not a dashboard, not a checklist, one step at a time.

### 2.1 The core loop

1. **Detect** — rules engine checks the business's current state against every applicable central + state threshold.
2. **Explain** — the relevant trigger is translated into plain vernacular language or voice, with no legal jargon.
3. **Guide** — the owner is given exactly one next action, with the documents already on file pre-filled.
4. **Re-detect** — once the action is done, the engine re-checks the business state and the loop continues.

This loop is the moat. Existing platforms are strong at step 3 (a one-off filing) but structurally absent from steps 1, 2, and 4 — they have no reason to alert a customer to a need they haven't already paid to solve.

### 2.2 Why this is one system with vast use cases, not a feature list

The use cases scale by adding rows to a maintained rule table — GST today, Karnataka Professional Tax next month, FSSAI the month after, labour codes after that — without re-architecting the product. The detection engine and the plain-language layer stay fixed; only the rule corpus grows. That corpus, kept accurate and current across states, is the defensible asset, more than the underlying code.

---

## 3. Product v1 (no AA/DEPA dependency)

v1 deliberately avoids Account Aggregator integration. It proves the detection-loop hypothesis on self-reported data first, because AA sandbox approval, DigiLocker integration, and bank-grade consent flows are a multi-month regulatory and engineering lift that should not gate the first proof of value.

### 3.1 Onboarding (5 minutes, voice or form, vernacular)

- Business type: sole proprietorship / partnership / LLP / Pvt Ltd / unregistered
- Sector: retail, food & beverage, services, manufacturing, trading, etc.
- State + city (state-specific thresholds differ materially)
- Approximate monthly revenue (range, not exact — lowers the trust barrier)
- Employee count
- Existing registrations already held (PAN, Udyam, GST, Shop License, etc.)

### 3.2 The rules engine

A maintained, versioned table of obligations, each row carrying: trigger condition, jurisdiction, the law/section it derives from, the filing deadline or window, the penalty for missing it, and the plain-language explanation template in each supported language. Examples of rows for the v1 corpus:

| Trigger | Jurisdiction | Obligation | Source |
|---|---|---|---|
| Annual turnover > ₹20L (₹10L for special category states) | Central | GST registration | CGST Act, Sec 22 |
| Any turnover, inter-state supply | Central | GST registration (no threshold) | CGST Act, Sec 24 |
| Investment/turnover within Udyam bands | Central | Udyam (MSME) classification update | MSME Dev Act |
| 10+ employees (varies by state) | State | Shops & Establishment registration | State Act |
| 20+ employees | Central | PF registration (EPFO) | EPF Act |
| 10+ employees, wage ≤ ceiling | Central | ESI registration | ESI Act |
| Manufacture/sale of food | Central/State | FSSAI license (basic/state/central by turnover) | FSSAI regs |
| Income/turnover varies | State | Professional Tax registration | State PT Act |

This table is the product's central nervous system. It should be owned by a compliance content team (not engineers) from day one, versioned, and auditable — every rule traceable to a specific section of law, with a last-verified date.

### 3.3 The alert + explanation layer

When a rule fires, the owner receives a short voice note or vernacular text message structured as: what changed, what it means for them specifically, what happens if ignored, and the one next step — never the full legal text, never a 12-item checklist.

### 3.4 What v1 deliberately does NOT do

- Does not file anything on the owner's behalf — it guides, and links out to the correct government portal with pre-filled-looking instructions
- Does not connect to Account Aggregators or DEPA — self-reported data only
- Does not give a legal opinion — every output is framed as informational guidance with a clear human-review path for anything filing-bound (see Section 6)

---

## 4. Roadmap: v1 → v2 → v3

| Phase | Unlocks | Key dependency |
|---|---|---|
| v1 — Self-reported detection | Prove the detect→explain→guide loop with zero integration risk | Rules corpus + vernacular NLP |
| v2 — Consented data via DEPA/AA | Auto-fill from verified bank/GST data; kills repetitive document upload | AA sandbox approval, FIU-IT registration |
| v3 — DPI rail integration | Compliance milestones unlock ONDC market access + OCEN credit eligibility automatically | Partnerships with ONDC/OCEN network participants |

---

## 5. Distribution: don't acquire one owner at a time

Micro-entrepreneurs are expensive and slow to reach directly. The realistic path is to embed inside organizations that already have daily relationships with this exact population:

- **CSC (Common Service Centre) operators** — already filing PAN/Udyam/GST for villages and small towns; white-label the alert engine into their existing workflow
- **Account Aggregator-enabled lending apps** — compliance status is a natural pre-qualifier for OCEN-based credit, giving lenders a reason to push this to borrowers
- **State MSME single-window portals** — a natural integration point since registration is already mandatory there
- **Tax-filing apps for small businesses** — white-label B2B2C distribution instead of competing for owner attention directly

---

## 6. The risk existing platforms hedge around: liability

Existing platforms upsell partly to fund human CA review, which also limits their legal exposure. An automated system that tells an owner whether they owe a filing sits close to the line of unlicensed legal/tax advice. This needs to be designed around from day one, not bolted on later:

- Frame every output as informational guidance based on self-reported data, never a determination — explicit, visible disclaimers, not buried in T&Cs
- Keep a human-in-the-loop / CA-marketplace handoff for anything that results in an actual filing, at least through v1 and v2
- Version and timestamp every rule with its legal source, so any specific alert is defensible and auditable
- Get the disclaimer and liability language reviewed by an actual lawyer before any public launch — this is the one piece of this plan that should not be DIY'd

---

## 7. 90-Day Build Plan

### Days 1–30 — Validate and scope the rules corpus

1. Pick exactly one state + 3 sectors (e.g. Jharkhand or Karnataka: retail, food service, small manufacturing) to go deep instead of wide
2. Build the v1 rule table for those sectors only — GST, Udyam, Shops & Establishment, Professional Tax, FSSAI as relevant. Every row traceable to a legal source.
3. Talk to 15–20 real micro-entrepreneurs in that state/sector — not surveys, sit with them, watch how they currently track (or fail to track) obligations
4. Draft the disclaimer/liability language with a lawyer in parallel — do not wait until launch

### Days 31–60 — Build the detect→explain→guide loop

1. Build the onboarding flow (voice + form) in at least 2 languages (e.g. Hindi + one regional language for the chosen state)
2. Build the rules engine as a simple condition-action table, not a hardcoded if/else — it must be editable by non-engineers from day one
3. Build the plain-language explanation templates per rule, per language — LLM-assisted drafting, human-reviewed for accuracy
4. Ship a WhatsApp-based MVP before a custom app — WhatsApp is where this audience already is, and it sidesteps app-install friction entirely

### Days 61–90 — Pilot and instrument

1. Recruit 50–100 real micro-entrepreneurs through one CSC partner or local trade association in the chosen state, not cold outreach
2. Instrument everything: did they understand the alert, did they take the next action, how long did it take, did they come back unprompted
3. Track one north-star metric: number of threshold events caught before a penalty/deadline, not signups or DAU
4. Use pilot data to decide the AA/DEPA integration case for v2 — only pursue it if self-reported data is proving to be the adoption bottleneck

---

*This is a working draft meant to be argued with, not a finished plan. The riskiest unverified assumption right now is whether self-reported data is trusted enough by owners to act on, versus needing the AA-verified version from day one. That should be the first thing the 15–20 conversations in Days 1–30 test directly.*
