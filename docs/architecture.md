# FieldView — Business & Impact Model (Back-of-Envelope)

FieldView targets the “**Misinformation Tax**”: the time and cognitive load fans spend sorting truth from clickbait in fast-moving sports news.

This model uses transparent assumptions and simple math so it can be refined with real user data.

---

## 1) Assumptions (India-focused)

- **Addressable market:** 1,000,000 highly engaged digital football fans in India
- **Time wasted today:** ~10 minutes/day/user consuming or verifying untrusted rumors
- **Time saved with FieldView:** up to 10 minutes/day/user via tier labels + one-click BS Filter  
  (upper-bound assumption; real value should be measured post-launch)

---

## 2) Time Saved (Core Impact)

**Total time reclaimed/day**
- 1,000,000 users × 10 min/day = **10,000,000 minutes/day**
- 10,000,000 minutes/day ÷ 60 = **166,666 hours/day**

**Conservative scenario (20% adoption)**
- 200,000 users × 10 min/day = 2,000,000 minutes/day
- = **33,333 hours/day**

---

## 3) Cost Reduction (Why AI verification scales)

### Baseline
Traditional verification relies on human moderators/editors or delayed community correction, which does not scale with rumor volume.

### Target design
FieldView’s planned pipeline verifies items automatically (LLM + Trust Dictionary).
- Claimed unit cost target: **~$0.0001/article** (assumption; depends on tokens, batching, and model pricing)

**Illustrative example**
- Verify 50,000 items/day:
  - Cost/day ≈ 50,000 × $0.0001 = **$5/day**
  - Cost/month ≈ **$150/month**

> Note: This repository is a frontend prototype; verification/ingestion is the next step.

---

## 4) Monetization (Premium “Noise-Free”)

- **Price:** ₹99/month
- **Conversion:** 2% of addressable market
  - 1,000,000 × 2% = **20,000 paying users**

**Monthly Recurring Revenue (MRR)**
- 20,000 × ₹99 = **₹1,980,000/month (~₹2.0M MRR)**

---

## 5) What Would Validate These Assumptions
To convert this model into measured impact, we would track:
- time-on-feed before/after using BS Filter
- % of sessions with BS Filter enabled
- retention lift from reduced misinformation exposure
- premium conversion rate and churn