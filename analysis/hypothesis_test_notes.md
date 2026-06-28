# Hypothesis Test Notes
## Campaign Onboarding & Activation Experiment

---

## 1. Metric Being Tested

**Metric:** Paid Conversion Rate  
**Definition:** Number of users who converted to paid plans ÷ Total users in group  
**Reason for selection:** Paid Conversion Rate is the North Star metric for this experiment. It directly measures whether the new campaign experience moves users from free/trial to paying customers, which is the primary business objective. This metric connects most directly to revenue and long-term company growth.

---

## 2. Hypotheses

**Null Hypothesis (H₀):**  
The paid conversion rate in the Treatment group is equal to or less than the paid conversion rate in the Control group.  
> H₀: p_treatment ≤ p_control

**Alternative Hypothesis (H₁):**  
The paid conversion rate in the Treatment group is greater than the paid conversion rate in the Control group.  
> H₁: p_treatment > p_control

---

## 3. Test Design

| Parameter | Value |
|-----------|-------|
| Test Type | Two-Proportion Z-Test |
| Tail | One-tailed (right-tailed) |
| Significance Level (α) | 0.05 |
| Confidence Level | 95% |

**Why one-tailed?**  
The business decision requires evidence that the Treatment is *better than* Control. We are not testing for any difference (two-tailed); we are specifically testing for improvement. A one-tailed test is appropriate and provides more statistical power in the direction of interest.

**Why a Z-Test?**  
With n > 30 in each group and binary outcome data (converted vs not), the two-proportion Z-test is appropriate. Both groups satisfy the conditions: n·p > 5 and n·(1-p) > 5.

---

## 4. Test Inputs

| Input | Value |
|-------|-------|
| Control Group Size (n₁) | 690 |
| Treatment Group Size (n₂) | 710 |
| Control Conversions (x₁) | 22 |
| Treatment Conversions (x₂) | 50 |
| Control Conversion Rate (p₁) | 3.19% |
| Treatment Conversion Rate (p₂) | 7.04% |
| Pooled Proportion (p̂) | 5.14% |

**Formula:**
```
Pooled proportion: p̂ = (x₁ + x₂) / (n₁ + n₂) = (22 + 50) / (690 + 710) = 0.0514

Standard Error: SE = sqrt(p̂ × (1 - p̂) × (1/n₁ + 1/n₂))
              SE = sqrt(0.0514 × 0.9486 × (1/690 + 1/710))
              SE = 0.01178

Z-statistic: z = (p₂ - p₁) / SE
             z = (0.0704 - 0.0319) / 0.01178
             z = 3.264
```

---

## 5. Test Output

| Output | Value |
|--------|-------|
| Z-Statistic | **3.264** |
| P-Value (one-tailed) | **0.000549** |
| Critical Value (α=0.05, one-tailed) | 1.645 |
| Decision | **Reject H₀** |

---

## 6. Decision Rule

- If p-value < α (0.05) → Reject H₀ → Treatment shows statistically significant improvement
- If p-value ≥ α (0.05) → Fail to Reject H₀ → Insufficient evidence for improvement

**Result:** p-value = 0.000549 < 0.05 → **Reject H₀**

---

## 7. Business Interpretation

The Treatment group (new campaign experience) achieved a **paid conversion rate of 7.04%**, compared to **3.19%** in the Control group. This represents:
- **Absolute lift:** +3.85 percentage points
- **Relative lift:** +120.7% improvement
- **Statistical significance:** p = 0.0005 (well below the 0.05 threshold)

The probability of observing this result by chance (if the treatment had no effect) is **0.055%** — extremely low. We have strong statistical evidence that the new onboarding campaign genuinely improves paid conversion rates.

**However**, this result must be evaluated alongside guardrail metrics before a full launch recommendation is made. The increase in support ticket rate (22% → 37.3%) and the decline in average revenue per converted user ($1,630 → $770) suggest the Treatment may be attracting lower-value users or creating post-conversion friction. These risks are analyzed in the recommendation memo.

---

## 8. Limitations

- Sample size is moderate (~700 per group); longer runtime could increase confidence further
- Experiment duration not specified — seasonal or temporal confounders unknown
- Revenue per converted user declined, which may offset gains from higher conversion volume
- Support ticket rate increase warrants root-cause investigation before full rollout
