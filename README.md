# Customer_Behavior_Analysis
Analysis with given data by internSpark
# Customer Segmentation, Retention & Churn-Risk Analysis

[![Open in Colab]
https://colab.research.google.com/drive/13D3KoTyCjsAasJiuvMegpxo734cAvSWl?usp=sharing

An end-to-end analysis of **250,000 e-commerce orders from 49,673 customers (Jan 2020 – Sep 2023, $681M revenue)**: data cleaning, feature engineering, RFM segmentation, purchase-pattern and cohort-retention analysis, behavioural churn-risk scoring, and five prioritised recommendations for improving customer engagement.

Built in Python (pandas, NumPy, SciPy, Matplotlib) as a step-by-step Colab notebook: 27 short cells, each ending in a check so any error is easy to locate.


## Business questions

1. Who are our customers, and which segments drive revenue?
2. How do customers buy over time (categories, payments, seasonality, repeat purchases)?
3. Are customers retained, and who is at risk of leaving?
4. What should the business do about it?

## Key findings

| Finding | Result |
|---|---|
| Revenue is concentrated | **Champions + Loyal customers = 33% of customers but 48% of revenue** |
| A large dormant base | **25% of customers silent for 365+ days** (a further 26% cooling at 181–365 days) |
| High-value customers slipping | "At Risk (high value)": 5,773 customers, ~$18.7K lifetime spend each, ~13 months since last order; with Hibernating they hold **$135M** of lifetime revenue |
| Repeat purchasing is slow | Median gap between orders is 148 days; only **28.6% reorder within 90 days** (96.6% reorder eventually) |
| Retention is flat, not decaying | About 28% of every cohort buys in each later quarter |
| No calendar seasonality | Month, weekday and hour are all within ±3% of average once the partial final month is excluded |
| Category and payment aren't levers | Average order value is nearly identical across categories and payment methods |

Segment shares, retention and conclusions were **replicated on a second, independent dataset** (`ecommerce_customer_data_large.csv`) with the same pipeline.

## Approach

**1. Cleaning & validation**
- Removed a duplicate column (`Customer Age` = `Age`, asserted in code) and the personal `Customer Name` column (non-unique, PII).
- Kept blank `Returns` values (19%) as *unknown* rather than imputing them.
- Checked for duplicates and invalid values; parsed dates; sorted by customer and time.
- **Found a data-integrity issue:** `Total Purchase Amount` matches price × quantity on only 0.02% of rows (correlation ≈ 0). Used the recorded total as revenue, and tested sensitivity: 79.5% of customers stay in the same segment if revenue is recomputed as price × quantity.

**2. Feature engineering (one row per customer)**
Recency, frequency, monetary value, average order value, tenure, average gap between orders, overdue ratio, return rate, category count, plus demographics.

**3. RFM segmentation**
Quintile scores (1–5) on recency, frequency and spend, mapped to nine named segments (Champions, Loyal, Potential Loyalists, New/Promising, Need Attention, About to Sleep, At Risk, Hibernating, Lost) and profiled by size, revenue share and behaviour.

**4. Retention & churn risk**
- Quarterly **cohort retention** heatmap (cohort = quarter of first purchase; the partial final quarter is masked so it isn't read as a drop).
- Time-to-second-purchase curve and inter-purchase gap distribution.
- **Behavioural risk tiers** from recency: Active (≤180 days), Cooling (181–365), Lapsed (>365).
- **Tested the dataset's own "Churn" flag** against every behavioural feature (point-biserial correlations, chi-square across segments): all correlations below 0.01 and p = 0.27, so the flag carries no learnable signal. Churn risk was therefore defined from purchase cadence instead of modelling the flag.

**5. Visualisation**
Nine charts saved to `outputs/`: revenue and active-customer trend, category/payment mix, seasonality, segment size vs revenue share, segment profile heatmap, cohort retention, purchase-gap and recency distributions, churn flag by segment, and risk tiers.

## Recommendations

1. **Tiered win-back program**, starting with At Risk (high value) customers: automated trigger at 181 days, stronger incentive at 365 days, cheap automated email for the Lost segment.
2. **Cadence-based triggers instead of calendar campaigns:** nudge around day 100–120, before the 148-day median gap, and flag customers beyond 2× their own gap (21% of repeat customers).
3. **Onboarding path for New / Promising and Potential Loyalists:** second-order incentive within 30 days (only 10.8% reorder that fast today).
4. **Reward and recruit through Champions and Loyal customers:** loyalty tiers and referrals; 94% of customers first bought by 2021, so existing advocates are the cheapest source of new customers.
5. **Lift basket size in low-AOV segments and fix measurement gaps:** test bundles/thresholds; reconcile order totals with price × quantity; investigate the ~50% recorded return rate; define churn properly; run every campaign with a holdout group.

## Limitations (stated up front)

- The data looks **synthetic**: most attributes are evenly distributed and unrelated to each other. Segment sizes and recency-based risk are dependable; claims about *what drives* behaviour (category, payment, demographics) are not supported.
- The customer base appears closed (~50,000 IDs), so the drop in "new customers" after 2021 may reflect that cap rather than weak demand.
- With real data I would add a predictive churn model (e.g. gradient boosting with time-based validation), uplift testing for the campaigns above, and customer lifetime value estimation.

## Skills demonstrated

Data cleaning and validation · feature engineering · RFM segmentation · cohort and retention analysis · hypothesis testing (point-biserial, chi-square) · sensitivity analysis · data visualisation · replication on a second dataset · translating findings into business actions · documenting limitations honestly.

## Repository structure

```
├── Alfido_Customer_Analysis_Colab.ipynb   # the analysis (run top to bottom)
├── README.md
├── outputs/                               # charts (PNG) and customer_segments_rfm.csv
└── data/                                  # not included: place the source CSVs here
```

## How to run

1. Click the **Open in Colab** badge (update the link with your username and repository name).
2. Run the cells in order. In Cell 2, upload `Customer_Behavior_Datasets.zip` when prompted.
3. To analyse the second file, change `FILE_NAME` in Cell 3 and run again.

Requires Python 3 with pandas, NumPy, SciPy and Matplotlib (all preinstalled in Colab).
