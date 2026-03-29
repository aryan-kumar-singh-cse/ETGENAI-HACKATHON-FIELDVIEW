# ⚽ FieldView — The AI‑Native News Experience (Frontend Prototype)

**Stop believing fake transfer news.** FieldView is a **frontend prototype** built for the **ET Gen AI Hackathon (Problem Statement #8: AI‑Native News Experience)**. It demonstrates how an AI-native sports feed *should* look and behave when every story is graded by trust.

This submission is intentionally **UI-first**: the feed uses **mocked stories (hardcoded JSON)** inside `fieldview.html` to simulate what a real ingestion + verification agent would output.

---

## 🚀 The Problem
Sports news on social media is optimized for engagement, not truth. Fans are flooded with rumors, aggregator spam, and clickbait. Manual verification is too slow for the pace of modern sports media.

---

## 💡 The Solution (Vision + What This Prototype Shows)

### Vision (Target System)
FieldView’s end-state is an **autonomous social listening + verification agent** that:
1. Ingests live rumors from social feeds
2. Verifies claims and grades source reliability
3. Outputs structured, tiered stories for the client to render
4. Lets users remove low-trust items instantly with the **BS Filter**

### Implemented in this repo (Prototype)
- Landing + mock auth + onboarding
- Club/league selection saved in `localStorage`
- Personalized feed UI:
  - Tier 1 / Tier 2 / Tier 3 styling
  - Accuracy badge
  - Quick Switch between clubs
  - Category tabs (Latest / Matches / Transfer / Squad / Club)
  - **BS Filter** toggle (hides Tier 3)
  - Search modal + Add-club modal
  - Load more stories

### Mocked in this repo (Prototype)
- Live ingestion and AI verification (no backend in this submission)

---

## 🛠️ Tech Stack
- **Frontend:** HTML5, CSS3, Vanilla JavaScript  
- **Storage:** Browser `localStorage`

---

## ⚙️ Setup / Run

### Option A — Open directly
1. Download/clone this repository
2. Open `fieldview.html` in your browser

### Option B (Recommended) — Run a local server
Some browsers restrict behavior when opening local HTML files directly. Run a tiny local server:

**Python**
```bash
python -m http.server 8000
```

Then open:
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
See `docs/architecture.md`.

---

## 📈 Impact Model
See `docs/impact-model.md`.

---

## 🗺️ Roadmap (Post‑Hackathon)
- Backend ingestion (Reddit/X/RSS) + normalization pipeline
- AI verification step (LLM + trust dictionary + claim checks)
- Real-time updates + notifications
- User accounts + cloud preference sync