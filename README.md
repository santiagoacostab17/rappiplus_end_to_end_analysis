# End-to-End Business Analysis

> From raw data to actionable insights — a full analytics pipeline covering data quality, profitability, user behavior, cohort retention, A/B testing, and BI dashboard delivery.

---

## Overview

This project evaluates the performance of the **RappiPlus** subscription service to support data-driven business decisions. The analysis follows a structured pipeline across six steps, transforming raw operational data into executive-ready insights and recommendations.

**Reviewed and approved** by TripleTen's data analytics program (2nd iteration — final approval ✅).

---

## Project Structure

```
rappiplus-end-to-end-analysis/
├── README.md
├── rappiplus_end_to_end_analysis.ipynb
├── data/
│   ├── orders_clean.csv
│   ├── catalog_clean.csv
│   └── marketing_clean.csv
└── dashboard/
    └── rappiplus_dashboards.pbix
```

---

## Data Sources

| Dataset | Description |
|---|---|
| `rappiplus_orders_raw.csv` | Orders, prices, discounts, and revenue |
| `rappiplus_catalog.csv` | Product costs, categories, and suppliers |
| `rappiplus_marketing_spend.csv` | Marketing investment by channel and country |
| `events` (SQL) | User behavior events across the purchase funnel |
| `users` / `user_activity` (SQL) | Registration dates and activity logs for cohort analysis |
| `experiment_checkout_ui.csv` | A/B test results for a checkout UI change |

---

## Analysis Pipeline

### Step 1 — Data Quality
Validated and cleaned all three CSV datasets: date parsing, numeric integrity checks (no negatives or invalid zeros), amount consistency validation (tolerance ±0.05), deduplication, and categorical normalization (strip, lowercase, dtype casting).

### Step 2 — Business Profitability
Calculated core KPIs by merging orders with product costs and marketing spend:
- Total revenue, total cost of goods, marketing investment, and net profit
- Average order ticket (with raincloud distribution plot)
- Units per order breakdown
- Top products by units sold
- Marketing spend by channel

### Step 3 — Conversion Funnel (SQL)
Queried the `events` table to track 7,796 unique users through six funnel stages:

| Stage | Key Finding |
|---|---|
| First visit → Select item | 2.74% drop — catalog retains attention well |
| Select item → Add to cart | Tracking anomaly detected (>100% conversion) — instrumentation issue |
| Add to cart → Begin checkout | 5.58% drop — typical cart abandonment |
| **Begin checkout → Add payment info** | **13.29% drop — largest friction point** |
| Add payment info → Purchase | 0.16% drop — near-zero friction at final step |

**Overall conversion: 80.04%**

### Step 4 — Cohort Retention (SQL)
Built a monthly cohort retention matrix using `users` and `user_activity` tables. Visualized as a heatmap showing retention percentages from month 0 through subsequent months by acquisition cohort.

### Step 5 — A/B Test (Statistical)
Evaluated the impact of a checkout UI redesign on conversion rate.

- **Test:** Two-proportion Z-test (two-tailed), α = 0.05
- **Effect size:** Cohen's h
- **Outcome:** Statistically significant result — the UI change impacts conversion rate

### Step 6 — BI Dashboard (Power BI)
Delivered two dashboards using the cleaned CSVs:

**Dashboard 1 — Executive Overview**
- KPI cards: Revenue, Profit, Marketing Spend, Avg Ticket
- Monthly revenue/profit trend lines (YTD)
- Revenue and profit by product/category

**Dashboard 2 — Drill-through Detail**
- Orders table with conditional formatting (profit positive/negative)
- Product-level sales breakdown
- Drill-through by product with filters by date and category

📎 [Dashboard Link (Google Drive)](https://drive.google.com/drive/folders/14Xlfw8Ee1aKzzKYU1tbqQhaG0woLcyJD?usp=sharing)

---

## Tech Stack

| Tool | Usage |
|---|---|
| Python | Core analysis (pandas, NumPy, SciPy, statsmodels, matplotlib, seaborn) |
| SQL (PostgreSQL) | Funnel analysis, cohort retention queries |
| SQLAlchemy | Python–database connection |
| Power BI | Executive and drill-through dashboards |
| Jupyter Notebook | Full pipeline documentation |

---

## Key Takeaways

1. **Highest-priority optimization:** The `Begin checkout → Add payment info` step loses 13% of users — reducing friction here has the highest revenue impact.
2. **Tracking issue:** The `select_item → add_to_cart` anomaly must be fixed before using funnel metrics for business decisions.
3. **UI test:** The checkout redesign produced a statistically significant change in conversion rate (Z-test, p < 0.05).
4. **Retention pattern:** Cohort analysis reveals how quickly RappiPlus users churn month-over-month, informing subscription retention strategies.

---

## Author

**Santiago Acosta**  
Data Analyst · TripleTen Bootcamp — Final Project  
[GitHub](https://github.com/santiagoacostab17) · [Portfolio](https://santiagoacostab17.github.io)
