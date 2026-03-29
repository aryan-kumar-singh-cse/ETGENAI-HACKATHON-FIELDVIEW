# FieldView — System Architecture & Agent Workflow (Prototype + Target Design)

This document describes (1) the **target agentic system** FieldView is designed to support and (2) what is **implemented in the current frontend-only prototype** submission.

---

## 1) System Diagram (Target Design)

```
[Live Internet Data]
   └─ Social/aggregator feeds (e.g., Reddit r/soccer/new.json)
            |
            v
     [Ingestion Agent]
   (Node.js + Axios poller)
            |
            v
 [Verification Brain / Reasoning Agent]
 (LLM step, e.g., Gemini / other)
   ├─ validates vs "Trust Dictionary"
   ├─ rewrites headline (clean)
   └─ produces reasoning/audit trail
            |
            v
      [Structured Output]
 (JSON: tier, cleaned_headline, reasoning,
  source, timestamp, club/category tags)
            |
            v
   [Client Application / UI Agent]
 (Vanilla JS renders tiered cards,
  personalization, BS Filter)
```

### Current submission note
In this repo, only the **Client Application** is implemented. The “Structured Output” is simulated using a **mock dataset** embedded in `fieldview.html` to demonstrate the full UI workflow.

---

## 2) Agent Roles & Communication (Target Design)

### A) Scraper / Ingestion Agent (Backend)
**Responsibility**
- Polls or subscribes to public feeds and extracts the newest unstructured posts.

**Output**
- Array of raw items: `{ title, url, timestamp, author/source hints }`.

**Communication**
- Sends raw items to the Verification Brain in batches.

---

### B) Reasoning Agent / Verification Brain (LLM + Rules)
**Responsibility**
- Constrained “editor-in-chief” pipeline:
  - Extract claimed journalist/source
  - Cross-reference a hardcoded **Trust Dictionary** (safe guardrails)
  - Assign a tier:
    - **Tier 1 (Green):** highly reliable / historically accurate
    - **Tier 2 (Yellow):** plausible but unconfirmed
    - **Tier 3 (Red):** low-trust, unknown, or clickbait patterns
  - Produce a cleaned headline and optional reasoning/audit trail

**Output**
- Structured JSON ready for the client.

---

### C) UI Agent (Frontend — Implemented in this Prototype)
**Responsibility**
- Renders tiered cards and engagement indicators
- Stores user onboarding preferences in `localStorage`
- Filters content instantly without round trips:
  - **BS Filter:** hides Tier 3 stories
  - Club and category filtering
- Search and quick-switch UI for navigation

**Communication**
- Prototype uses `localStorage` as its state store:
  - `fieldview_user`
  - `fieldview_onboarding_complete`
  - `fieldview_selected_clubs`
  - `fieldview_bs_filter`

---

## 3) Error Handling & Safe-Fail Logic

### Target design (backend + LLM)
- **LLM/API failures:** wrap in `try/catch`. If an item fails verification/parsing, return `null` for that item and drop it from the final array so the feed never crashes.
- **Source not found:** if journalist/source is absent from Trust Dictionary, default to **Tier 3** (safe-fail).

### Prototype (frontend-only) equivalents (implemented)
- **localStorage parse safety:** guarded parsing in `getUser()`
- **empty feed state:** renders “No stories found” rather than breaking
- **image failures:** `onerror` shows a placeholder instead of a broken UI
- **modal safety:** ESC key and background click close modals

---

## 4) Key Interface (Target)

Example response contract from `/api/news`:
```json
{
  "items": [
    {
      "id": "string",
      "tier": 1,
      "source": "string",
      "headline": "string",
      "reasoning": "string",
      "category": "TRANSFER|MATCH|SQUAD|CLUB",
      "club": "string",
      "timestamp": "ISO-8601 string"
    }
  ]
}
```

The UI is designed so the backend/LLM pipeline can improve without redesigning the client.