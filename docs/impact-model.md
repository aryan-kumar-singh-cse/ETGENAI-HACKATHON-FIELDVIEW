# FieldView — Impact Model (Eradicating the Misinformation Tax)

FieldView reduces the “**Misinformation Tax**”: time + cognitive load wasted on fake, clickbait, or low-trust sports news.

This is a back-of-envelope model with explicit assumptions (India-focused). Numbers should be validated with post-launch analytics.

---

## 1) Assumptions

### Market
- **Addressable market (India):** 1,000,000 highly engaged digital football fans

### Current pain
- **Time wasted today:** ~10 minutes/day/user
  - reading rumors
  - cross-checking sources
  - emotionally reacting to false claims

### FieldView impact (Target system)
- **Time saved:** up to 10 minutes/day/user
  - driven by tiering + the **BS Filter** removing Tier 3 noise instantly
  - safe-fail defaults (unknown source → Tier 3)

> Prototype note: the UI shipped here demonstrates the workflow and controls; the ingestion/LLM verification is part of the target deployment design.

---

## 2) Time Reclaimed (Core Benefit)

**Upper-bound scenario**
- 1,000,000 users × 10 min/day = **10,000,000 minutes/day**
- ÷ 60 = **166,666 hours/day**

**Conservative adoption scenario (20%)**
- 200,000 users × 10 min/day = 2,000,000 minutes/day
- ÷ 60 = **33,333 hours/day**

---

## 3) Cost Reduction (Why Agentic Verification Scales)

### Traditional approach
Human editorial verification or community corrections are slow and expensive at scale.

### FieldView approach (Target)
Automated verification via LLM + Trust Dictionary guardrails.
- Working assumption: **~$0.0001 per article** (depends on tokens/model/batching)

**Illustrative cost**
- Verify 50,000 candidate items/day:
  - Cost/day ≈ 50,000 × $0.0001 = **$5/day**
  - Cost/month ≈ **$150/month**

This makes “editorial-grade filtering” feasible without hiring large moderation teams.

---

## 4) Monetization (Premium “Noise‑Free” Experience)

### Premium subscription model
- **Price:** ₹99/month
- **Conversion:** 2% of addressable market
  - 1,000,000 × 2% = **20,000 users**

**MRR**
- 20,000 × ₹99 = **₹1,980,000/month (~₹2.0M MRR)**

---

## 5) Validation Metrics (What we’d measure post-launch)
- BS Filter usage rate (% sessions with filter ON)
- average time spent per session before/after tiering
- retention (D1/D7/D30) lift for users who enable BS Filter
- premium conversion and churn
- distribution of engagement by tier (Tier 1 vs Tier 3)