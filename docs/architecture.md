# FieldView — System Architecture & Agent Workflow

This document describes the **full agentic architecture** of FieldView and how agents communicate, integrate with tools, and fail safely.

**Current repo status (2026-03-29):** Frontend prototype only. Backend/LLM agents are described as the **target deployment design**.

---

## 1) System Diagram (Target Deployment)

```
[Live Internet Data]
   └─ Reddit r/soccer/new.json (public JSON feed)
            |
            v
[Ingestion Agent / Scraper]
  Node.js + Axios
  - fetch top N new posts
  - normalize to raw items
            |
            v
[Verification Brain / Reasoning Agent]
  LLM (e.g., Gemini)
  - extract cited journalist/source
  - validate vs Trust Dictionary (hardcoded)
  - assign Tier (1/2/3)
  - rewrite headline
  - produce reasoning/audit trail
            |
            v
[Structured Output Contract]
  JSON items:
  { tier, source, cleaned_headline, reasoning, club, category, ts }
            |
            v
[Client Application / UI Agent]
  Vanilla JS frontend
  - render tiered cards (green/yellow/red)
  - personalization (clubs/leagues)
  - BS Filter hides Tier 3 client-side
```

### Prototype mapping (this repo)
- The **Client Application / UI Agent** is implemented in `fieldview.html`.
- The **Structured Output Contract** is simulated via a hardcoded mock dataset (`ALL_NEWS`) to demonstrate the intended UX.

---

## 2) Agent Roles & Responsibilities

### A) Ingestion Agent (Scraper Agent)
**Goal:** Convert chaotic “internet firehose” data into a clean list of candidate items for verification.

**Responsibilities**
- Poll Reddit feed (e.g., `r/soccer/new.json`) on an interval or on-demand.
- Extract fields (title, permalink, timestamp).
- Deduplicate and drop obvious spam patterns.

**Output**
- `raw_items[]` with unstructured text.

**Tool integrations**
- HTTP client (Axios/fetch)
- (Optional) cache layer to avoid rate limits

---

### B) Verification Brain (Reasoning Agent / Digital Editor-in-Chief)
**Goal:** Produce safe, structured, trust-scored outputs.

**Responsibilities**
- Prompted to operate under strict constraints:
  1. Identify cited journalist/source (or mark “unknown”)
  2. Cross-check against **Trust Dictionary** (hardcoded mapping of source → tier and/or accuracy)
  3. Assign Tier:
     - **Tier 1:** verified/high trust
     - **Tier 2:** plausible/needs confirmation
     - **Tier 3:** low trust / unknown / clickbait patterns (safe default)
  4. Rewrite headline into consistent, clean format
  5. Generate a short reasoning/audit trail (for transparency)

**Output**
- `verified_items[]` (strict JSON contract)

**Tool integrations**
- LLM API (e.g., Gemini)
- Trust Dictionary (local object/file)

---

### C) UI Agent (Client Application)
**Goal:** Present trust signals clearly and let users control noise.

**Responsibilities (implemented)**
- Render cards with tier colors and accuracy badges.
- Provide controls:
  - club quick-switch
  - category tabs
  - search and add-club flows
  - **BS Filter** (hide Tier 3 immediately)
- Persist preferences in `localStorage`.

**Data dependencies**
- Expects a stable JSON shape (whether mocked or delivered by backend).

---

## 3) Communication & Contracts

### Target contract: `/api/news`
The backend returns:
```json
{
  "items": [
    {
      "id": "string",
      "tier": 1,
      "source": "string",
      "headline": "string",
      "reasoning": "string",
      "category": "LATEST|MATCH|TRANSFER|SQUAD|CLUB",
      "club": "string",
      "timestamp": "ISO-8601"
    }
  ]
}
```

### Prototype contract (this repo)
The same structure is simulated in the `ALL_NEWS` dataset inside `fieldview.html`.

---

## 4) Error Handling & Safe-Fail Logic

### A) API timeouts / LLM failures (Target)
- Wrap verification calls in `try/catch`.
- If an item fails verification/parsing:
  - return `null` for that item
  - drop it from results: `items.filter(x => x !== null)`
- The feed should **never crash** due to a single bad post.

### B) Source not found (Target)
- If no cited journalist/source is detected or it’s missing from Trust Dictionary:
  - **default to Tier 3** (safe-fail)
This protects the user against unverified information.

### C) Client-side resilience (Prototype implemented)
- safe localStorage parsing
- empty state UI when no results
- image error fallbacks
- modal close logic (ESC / background click)

---

## 5) Security & Abuse Considerations (Target)
- Rate limiting on ingestion endpoints
- Caching and deduplication
- Input sanitization (treat all external text as untrusted)
- Logging of verification failures for monitoring