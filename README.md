# ⚽ FieldView — The AI‑Native News Experience (Frontend Prototype + Full Agent Design)

**Stop believing fake transfer news.** FieldView is an AI‑native sports news experience built for the **ET Gen AI Hackathon (Problem Statement #8: AI‑Native News Experience)**.

This repository contains a **frontend prototype** that demonstrates the complete product experience (tiers, onboarding, personalization, BS Filter). It also documents the **full agentic system** FieldView is designed to run once deployed.

**Status (as of 2026-03-29):**
- ✅ Frontend prototype shipped (this repo)
- 🧩 Backend ingestion + LLM verification pipeline documented (target design; not deployed in this repo)

---

## 🚀 The Problem
Sports media on social platforms is optimized for engagement, not truth. Fans are flooded with clickbait and unverified rumors, wasting time and mental energy verifying what’s real.

---

## 💡 The Solution (Full Agent Workflow)

FieldView is designed as an **Autonomous Social Listening + Verification Agent** that turns chaotic internet rumors into a clean, trustworthy feed.

### End-to-end pipeline (Target / Planned)
1. **Ingest (Scraper Agent):** Continuously monitors public social feeds (e.g., Reddit `r/soccer/new.json`) and extracts the newest unstructured posts.
2. **Verify (Reasoning Agent / LLM):** Sends raw headlines to an LLM (e.g., Gemini) that:
   - extracts the cited journalist/source
   - cross-references a **hardcoded Trust Dictionary**
   - assigns a trust **Tier (1/2/3)**
   - rewrites the headline into a clean format
   - generates a short reasoning/audit trail
3. **Structure:** Produces a strict JSON contract the client can render safely.
4. **Display (UI Agent):** The frontend renders color-coded cards and enables client-side controls like the **BS Filter** (hide Tier 3 instantly).

### What is implemented in this repo (Prototype)
- Landing + mock auth + onboarding
- Club/league selection saved in `localStorage`
- Feed UI with:
  - Tier 1 / Tier 2 / Tier 3 styling
  - Accuracy badge
  - Quick Switch between clubs
  - Category tabs (Latest / Matches / Transfer / Squad / Club)
  - **BS Filter** toggle (hides Tier 3)
  - Search modal + Add-club modal
  - Load more stories
- Feed content uses **mocked stories (hardcoded JSON)** to simulate the agent output.

---

## 🛠️ Technology Stack
### Prototype (this repo)
- **Frontend:** HTML5, CSS3, Vanilla JavaScript
- **Storage:** Browser `localStorage`

### Target deployment (documented design)
- **Ingestion Agent:** Node.js + Axios polling
- **API Layer:** Node.js + Express
- **Verification Brain:** LLM (e.g., Gemini) + Trust Dictionary guardrails

---

## ⚙️ Setup / Run (Prototype)

### Option A — Open directly
1. Download/clone this repository
2. Open `fieldview.html` in your browser

### Option B (Recommended) — Run a local server
```bash
python -m http.server 8000
```

Open:
- `http://localhost:8000/fieldview.html`

---

## 🧭 Demo Checklist
1. Create Account (any email/password)
2. Complete onboarding (select clubs/leagues)
3. In the feed:
   - Toggle **BS Filter** ON/OFF
   - Switch clubs via **Quick Switch**
   - Use **Search (🔍)** to jump to a club
   - Use **+ Add** to add clubs to Quick Switch
   - Click **Load more stories**

---

## 🧱 Architecture
See `docs/architecture.md` (includes system diagram, agent roles, communication, and error handling).

---

## 📈 Impact Model
See `docs/impact-model.md` (time saved + monetization assumptions).

---

## 🗺️ Roadmap (Post‑Hackathon)
- Implement Node.js ingestion agent (Reddit/X/RSS)
- Integrate LLM verification with safe-fail defaults and audit trails
- Replace mock dataset with `/api/news` output
- Real-time updates + notifications
- Accounts + sync across devices