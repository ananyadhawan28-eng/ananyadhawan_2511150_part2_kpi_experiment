# Recommendation Memo
## Campaign Onboarding & Activation Experiment

**To:** Leadership / Product Decision Makers  
**From:** Business Analyst  
**Date:** June 2026  
**Subject:** Launch Recommendation — New Onboarding & Activation Campaign

---

## 1. Executive Summary

The new onboarding and activation campaign (Treatment) produced a **statistically significant 120.7% improvement** in paid conversion rate (3.19% → 7.04%, p = 0.0005), well above our significance threshold of α = 0.05. Funnel progression metrics improved across all stages.

However, guardrail metrics reveal meaningful risks: the support ticket rate surged from 22.0% to 37.3%, and average revenue per converted user dropped from $1,630 to $770 — suggesting the new campaign attracts higher volumes of lower-quality conversions while creating post-conversion friction.

**Recommendation: Conditional Launch — proceed with the Treatment for Free plan users while pausing for Basic and Premium plan users, and implement a parallel effort to investigate the support ticket surge.**

---

## 2. North Star Metric

**Selected North Star: Paid Conversion Rate**  
(Converted Users ÷ Total Users in Group)

### Why This Metric?

Paid Conversion Rate is the most direct measure of whether the onboarding campaign achieves its core objective: moving users from free/trial status to paying customers. It:

- Directly links user activation to revenue generation
- Measures the full funnel impact, not just a single step
- Is unambiguous: a user either pays or does not

### Why Not Other Metrics?

| Metric | Why Not North Star |
|--------|-------------------|
| Landing Page Visit Rate | Top-of-funnel only; doesn't confirm intent or value |
| Engagement Score | A proxy for interest, not commitment |
| Trial Start Rate | Users can trial without converting |
| Revenue Per User | Confounded by number of users; doesn't reflect campaign effectiveness directly |

### Risk of Blind Optimization

If optimized in isolation, the campaign might draw more low-intent users who convert initially but churn quickly, inflate support costs, or request refunds — hollow growth. Guardrail metrics must be monitored in parallel.

---

## 3. KPI Tree Summary

```
[NORTH STAR] Paid Conversion Rate
        |
        +---- DRIVER 1: Funnel Progression
        |          - Landing Page Visit Rate  (63.6% -> 72.4%)
        |          - Trial Start Rate         (25.1% -> 29.0%)
        |          - Onboarding Completion    (15.7% -> 21.1%)
        |
        +---- DRIVER 2: Revenue Quality
        |          - Avg Revenue / User       ($51.97 -> $54.25)
        |          - Avg Revenue / Converted  ($1,630 -> $770)  [RISK]
        |          - Refund Rate              (0.00% -> 0.42%)  [Monitor]
        |
        +---- DRIVER 3: User Experience
                   - Avg Engagement Score    (57.0 -> 62.9)
                   - Days to Convert         (8.9 -> 6.4 days)
                   - Support Ticket Rate     (22.0% -> 37.3%)  [RISK]

GUARDRAIL METRICS
    - Support Ticket Rate        [High Risk]
    - Avg Revenue/Converted User [High Risk]
    - Refund Rate                [Monitor]
```

See `outputs/kpi_tree.png` for the full visual KPI tree.

---

## 4. Experiment Result Summary

| Metric | Control | Treatment | Lift |
|--------|---------|-----------|------|
| User Count | 690 | 710 | — |
| Landing Page Visit Rate | 63.6% | 72.4% | +8.7pp |
| Trial Start Rate | 25.1% | 29.0% | +3.9pp |
| Onboarding Completion Rate | 15.7% | 21.1% | +5.5pp |
| **Paid Conversion Rate** | **3.19%** | **7.04%** | **+3.85pp (+121%)** |
| Avg Revenue / User | $51.97 | $54.25 | +4.4% |
| Avg Revenue / Converted User | $1,630 | $770 | **-52.8%** |
| Refund Rate | 0.00% | 0.42% | +0.42pp |
| Support Ticket Rate | 22.0% | 37.3% | **+15.3pp** |
| Avg Engagement Score | 57.0 | 62.9 | +10.4% |
| Avg Days to Convert | 8.9 days | 6.4 days | -2.5 days |

---

## 5. Hypothesis Test Interpretation

**Test:** Two-Proportion Z-Test (one-tailed, α = 0.05)  
**H₀:** p_treatment ≤ p_control  
**H₁:** p_treatment > p_control

| Parameter | Value |
|-----------|-------|
| Z-Statistic | 3.264 |
| P-Value | 0.0005 |
| Critical Value | 1.645 |
| Decision | **Reject H₀** |

The p-value of 0.0005 is far below α = 0.05. We have **very strong statistical evidence** that the new campaign improves paid conversion rates. The probability this result occurred by chance is 0.055%.

---

## 6. Guardrail Metric Analysis

### 6.1 Support Ticket Rate [HIGH RISK]

- **Control:** 22.0% | **Treatment:** 37.3% | **Increase:** +15.3 percentage points (+69.5%)
- The Treatment group generates 69% more support requests per user, consistent across all four regions (North: +14pp, West: +21pp, East: +15pp, South: +11pp)
- **Risk:** This signals that the new onboarding experience may confuse users, set incorrect expectations, or onboard users who need more hand-holding. At scale, this drives significant customer support costs and risks user churn and reputation damage.
- **Action Required:** Investigate root causes before full launch. Analyze ticket categories to identify specific onboarding pain points.

### 6.2 Avg Revenue Per Converted User [HIGH RISK]

- **Control:** $1,630 | **Treatment:** $770 | **Decline:** -52.8%
- Although more users converted in the Treatment group, the revenue per converted user declined sharply. This suggests the new experience may be attracting lower-intent or lower-value users — users who convert at a lower price point or downgrade quickly.
- **Risk:** Volume gains may not offset revenue quality losses over a longer time horizon. LTV (Lifetime Value) could be significantly impacted.
- **Calculation check:** Control total conversion revenue ≈ 22 × $1,630 = $35,860. Treatment: 50 × $770 = $38,500. Marginal revenue gain is +$2,640 on 710 users — a very thin margin that support cost increases could easily erase.

### 6.3 Refund Rate [MONITOR]

- **Control:** 0.00% | **Treatment:** 0.42%
- Small absolute increase but directionally concerning. At scale (e.g., 100,000 users), a 0.42% refund rate = 420 refunds.
- **Action Required:** Monitor refund rates continuously if launched.

---

## 7. Segment-Level Insights

### By Region
All four regions show conversion improvement; North performs strongest (+5.4pp lift, highest absolute conversion at 8.9% in Treatment). West shows smallest lift (+1.7pp).

### By Device Type
Mobile users show the highest relative lift (+4.8pp), suggesting the new campaign resonates particularly well with mobile-first users.

### By Traffic Source
**Referral users** show the strongest conversion in Treatment (11.0% vs. 2.5% in Control — a 4.4× improvement). **Paid Search** users also show strong improvement (6.3% vs. 1.3%).

### By Plan Type
**Free plan users** drive the majority of the lift (9.3% vs. 3.1% in Control, +6.2pp). **Basic plan users** show virtually no improvement (3.9% vs. 3.6%), which is noteworthy: the campaign appears ineffective for Basic plan users but highly effective for Free users.

**Strategic implication:** A targeted rollout to Free plan users (particularly from Referral and Paid Search channels) would capture most of the conversion benefit while limiting exposure to guardrail risks.

---

## 8. Final Recommendation

### Decision: CONDITIONAL LAUNCH

**Launch the Treatment for Free plan users immediately. Hold for Basic and Premium users pending investigation.**

**Rationale:**
1. The conversion improvement is statistically significant and economically meaningful for Free plan users
2. Basic plan users see no meaningful benefit from the Treatment (+0.3pp), making the risk/reward unattractive
3. The support ticket and revenue quality risks are real and require root-cause analysis before a full rollout
4. A phased approach limits downside while capturing significant upside

**Phased Rollout Plan:**

| Phase | Scope | Trigger to Advance |
|-------|-------|-------------------|
| Phase 1 (Immediate) | Free plan users | Conversion holds ≥ 7%, support ticket rate monitored |
| Phase 2 (4–6 weeks) | Free + Referral + Paid Search | Support ticket increase resolved, revenue quality stable |
| Phase 3 (8–12 weeks) | Full rollout | All guardrail metrics within acceptable thresholds |

---

## 9. Risks and Limitations

1. **Revenue quality risk:** The 53% drop in revenue per converted user is the single biggest concern. If converted users churn faster or are on lower-value plans, long-term revenue impact could be negative.
2. **Support cost risk:** A 69% increase in support tickets at scale represents a significant operational cost that could nullify conversion gains.
3. **Experiment duration unknown:** Without knowing how long the experiment ran, we cannot rule out seasonal or temporal confounding.
4. **No LTV data:** We have only 30-day revenue data. The true impact on long-term customer value is unknown.
5. **Sample size:** With ~700 users per group and only 72 conversions total, subgroup analyses (device, traffic, plan) carry higher variance.
6. **Refund rate:** Currently small but directionally negative; requires ongoing monitoring.

---

## 10. Next Steps

1. **Root-cause analysis on support tickets** — categorize ticket types to identify onboarding friction points
2. **Phase 1 launch for Free plan users** with daily monitoring of support ticket rate and refund rate
3. **LTV tracking** — set up 90-day and 180-day revenue tracking for both groups
4. **Qualitative research** — user interviews or surveys with converted Treatment users to understand satisfaction and intent to retain
5. **Optimize the campaign** to address support ticket drivers before Phase 2 expansion
6. **Re-run hypothesis test** on segment-specific data for Basic plan users to determine if no effect is statistically confirmed

---

*Analysis based on campaign_experiment_data.xlsx — 1,400 users after data cleaning. Statistical test: Two-Proportion Z-Test, α = 0.05.*
