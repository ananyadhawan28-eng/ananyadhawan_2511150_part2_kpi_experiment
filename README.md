# Campaign Onboarding & Activation Experiment
## Business Analytics Assignment — Full Analysis Repository

---

## Business Context

A subscription-based digital product company launched a new onboarding and activation campaign to improve user conversion and early engagement. Users were randomly divided into two groups:

- **Control Group:** Existing onboarding experience
- **Treatment Group:** New campaign experience

Leadership wants to determine whether the Treatment should be launched to all users. This repository contains the full analysis: data cleaning, KPI framework, statistical testing, guardrail evaluation, and a data-backed recommendation.

---

## Dataset Description

**File:** `data/campaign_experiment_data.xlsx`  
**Source:** Company experiment system  
**Records:** 1,408 raw rows → 1,400 after cleaning  

| Column | Type | Description |
|--------|------|-------------|
| user_id | String | Unique user identifier |
| signup_date | Date | User signup date (Excel serial number) |
| experiment_group | String | "Control" or "Treatment" |
| region | String | North, South, East, West |
| device_type | String | Desktop, Mobile, Tablet |
| traffic_source | String | Email, Organic, Paid Search, Referral, Social |
| plan_type | String | Free, Basic, Premium |
| visited_landing_page | Binary (0/1) | Did user visit landing page? |
| started_trial | Binary (0/1) | Did user start a trial? |
| completed_onboarding | Binary (0/1) | Did user complete onboarding? |
| converted_to_paid | Binary (0/1) | Did user convert to paid plan? |
| revenue_30d | Float | Revenue generated in 30 days ($) |
| support_tickets_30d | Integer | Number of support tickets raised |
| refund_requested | Binary (0/1) | Did user request a refund? |
| days_to_convert | Float | Days from signup to conversion (null for non-converters) |
| engagement_score | Float | Composite engagement score (0–100) |

---

## North Star Metric

**Selected: Paid Conversion Rate** (Converted Users ÷ Total Users)

This metric was selected because it most directly measures whether the campaign achieves its business objective — moving users from free/trial to paying customers. It ties directly to revenue generation and sustainable growth.

Other metrics (engagement score, trial start rate, landing page visits) are important leading indicators but are not the final measure of campaign success.

**Risk of blind optimization:** High conversion volume with low revenue per user or high churn could lead to hollow growth. Guardrail metrics are therefore essential.

---

## KPI Tree Summary

```
[NORTH STAR] Paid Conversion Rate
    |
    +-- DRIVER 1: Funnel Progression
    |       - Landing Page Visit Rate
    |       - Trial Start Rate
    |       - Onboarding Completion Rate
    |
    +-- DRIVER 2: Revenue Quality
    |       - Avg Revenue Per User
    |       - Avg Revenue Per Converted User  [GUARDRAIL]
    |       - Refund Rate                     [GUARDRAIL]
    |
    +-- DRIVER 3: User Experience
            - Avg Engagement Score
            - Days to Convert
            - Support Ticket Rate             [GUARDRAIL]
```

See `outputs/kpi_tree.png` for the full visual diagram.

---

## Data Cleaning Documentation

All cleaning steps are documented in `analysis/experiment_analysis.xlsx` → "Data Quality Log" sheet.

| Issue | Finding | Resolution |
|-------|---------|------------|
| Duplicate user_ids | 8 duplicate pairs (16 rows) | Kept first occurrence; removed 8 rows |
| Missing: device_type | 18 null values | Imputed as 'Unknown' category |
| Missing: traffic_source | 24 null values | Imputed as 'Unknown' category |
| Missing: engagement_score | 14 null values | Imputed with group-level median |
| Missing: days_to_convert | 1,328 nulls | Expected (non-converters have no conversion date) — retained as N/A |
| Binary column validity | 0 invalid values across 5 binary columns | All confirmed 0 or 1 |
| Revenue outliers (>$1,000) | 24 users | All are genuine paid converters — retained |
| Group balance | Control: 690, Treatment: 710 | ~1.5% imbalance — acceptable |
| Segment distribution | Region, Device, Traffic, Plan | Balanced across groups — no confounding |

**Final dataset: 1,400 users (690 Control, 710 Treatment)**

---

## Experiment Analysis Approach

1. **Funnel analysis:** Measured conversion at each funnel stage (Landing Page → Trial → Onboarding → Paid)
2. **Metric calculation:** Computed all 11 required metrics for Control vs. Treatment
3. **Segment analysis:** Broken down by Region, Device Type, Traffic Source, and Plan Type
4. **Guardrail evaluation:** Evaluated support tickets, refund rate, and revenue quality as risk signals
5. **Statistical testing:** Applied a two-proportion Z-test on the North Star metric

---

## Hypothesis Test Summary

**Test:** Two-Proportion Z-Test (one-tailed, H₁: p_treatment > p_control)  
**Metric Tested:** Paid Conversion Rate  
**Significance Level:** α = 0.05

| Input | Value |
|-------|-------|
| Control conversion | 3.19% (22/690) |
| Treatment conversion | 7.04% (50/710) |
| Z-Statistic | 3.264 |
| P-Value | 0.0005 |
| Decision | **Reject H₀ — Statistically Significant** |

The treatment group shows a statistically significant improvement in paid conversion rate with 99.9% confidence.

Full test notes: `analysis/hypothesis_test_notes.md`

---

## Guardrail Metrics Considered

| Guardrail Metric | Control | Treatment | Status |
|-----------------|---------|-----------|--------|
| Support Ticket Rate | 22.0% | 37.3% | **HIGH RISK — +69% increase** |
| Avg Revenue / Converted User | $1,630 | $770 | **HIGH RISK — -53% decline** |
| Refund Rate | 0.00% | 0.42% | **MONITOR — early signal** |

These guardrail results prevent a simple full-launch recommendation despite the strong conversion result.

---

## Final Recommendation

**CONDITIONAL LAUNCH**

- Launch Treatment immediately for **Free plan users** (largest benefit: +6.2pp lift)
- Hold for **Basic plan users** (no meaningful benefit observed: +0.3pp)
- Resolve support ticket root cause before expanding to Premium and full user base
- Monitor revenue per converted user and refund rates continuously

Full reasoning: `outputs/recommendation_memo.md`

---

## Assumptions and Limitations

1. Users were randomly assigned — experiment design integrity assumed, not verified
2. 30-day revenue is used as a proxy for LTV; actual long-term impact is unknown
3. Experiment duration is not specified in the dataset; seasonal effects cannot be ruled out
4. Support ticket categories are unknown; root cause of increase cannot be determined from this data alone
5. Revenue outliers (>$1,000) were retained as legitimate high-value converters
6. Missing device_type and traffic_source values were imputed as 'Unknown', not excluded
7. Chi-square tests for segment-level statistical significance were not performed due to small sub-group sizes

---

## File Structure

```
project/
├── data/
│   └── campaign_experiment_data.xlsx      
├── analysis/
│   ├── experiment_analysis.xlsx            
│   └── hypothesis_test_notes.md           
├── outputs/
│   ├── experiment_summary.xlsx           
│   ├── recommendation_memo.md             
│   └── kpi_tree.png                       
├── screenshots/
│   ├── summary_metrics.png                
│   ├── hypothesis_test_output.png         
│   └── kpi_tree_preview.png              
└── README.md                            
```

---

## Screenshots

| Screenshot | Contents |
|-----------|---------|
| `screenshots/summary_metrics.png` | Full Control vs Treatment metric comparison table |
| `screenshots/hypothesis_test_output.png` | Z-test distribution plot and results summary |
| `screenshots/kpi_tree_preview.png` | KPI tree showing North Star → Drivers → Sub-Drivers → Guardrails |
