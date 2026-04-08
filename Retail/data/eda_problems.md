# EDA Problems: Philippine Retail Chain Sales Dataset

**Dataset:** `retail_sales.csv`  
**Rows:** 3,500 | **Columns:** 38  
**Context:** Simulated transaction-level data from a multi-format retail chain operating across 8 Philippine regions (2022–2024). Not real company data — built for EDA practice.

---

## Dataset Schema

| Column | Type | Description |
|---|---|---|
| `transaction_id` | str | Unique transaction key |
| `order_date` | str | Date of transaction (YYYY-MM-DD) |
| `year` | int | 2022, 2023, 2024 |
| `month` | int | 1–12 |
| `quarter` | str | Q1–Q4 |
| `store_id` | str | One of 20 store IDs |
| `store_format` | str | Superstore / Express / Online / Kiosk |
| `region` | str | 8 regions |
| `sales_channel` | str | In-Store / Website / Mobile App / Third-Party Platform |
| `employee_id` | str | Assigned sales employee |
| `customer_id` | str | **Has nulls** — anonymous in-store transactions |
| `customer_segment` | str | New / Regular / VIP / At-Risk / Churned |
| `category` | str | 8 product categories |
| `subcategory` | str | Sub-level of category |
| `product_id` | str | One of 200 product IDs |
| `supplier` | str | 15 suppliers (Supplier_A through Supplier_O) |
| `base_price_php` | float | Original list price |
| `unit_cost_php` | float | Cost per unit from supplier |
| `discount_pct` | int | Applied discount percentage (0–50) |
| `selling_price_php` | float | Final price after discount |
| `quantity_sold` | int | Units sold per transaction |
| `gross_revenue_php` | float | selling_price × quantity |
| `cogs_php` | float | unit_cost × quantity |
| `gross_profit_php` | float | gross_revenue − cogs |
| `gross_margin_pct` | float | Gross profit as % of revenue |
| `shipping_cost_php` | float | 0 for in-store; varies for online |
| `delivery_days` | int | 0 for in-store; days to deliver otherwise |
| `promo_type` | str | None / Percent Discount / Buy1Get1 / Flash Sale / Loyalty Points / Bundle Deal |
| `payment_method` | str | GCash / Credit Card / Cash / etc. |
| `return_flag` | int | 1 if item was returned |
| `return_reason` | str | Reason for return (None if not returned) |
| `stock_level` | int | Inventory units available at time of sale |
| `reorder_point` | int | Threshold that triggers restocking |
| `stockout_flag` | int | 1 if stock_level < reorder_point |
| `customer_rating` | float | 1.0–5.0; **~7% null** |
| `nps_score` | float | 0–10 Net Promoter Score; **~36% null** (not always collected) |
| `loyalty_points_earned` | int | Points credited per transaction |
| `marketing_spend_php` | float | Attributed marketing cost per transaction |

---

## Problems

### Section 1 — Business Overview (Stakeholder / Executive Level)

**Q1. [Board Report]**
The CEO wants a single-slide revenue summary for the board. Compute total gross revenue, total gross profit, and average gross margin across all years. Then break these down by `year`. Has the business grown year-over-year? What is the YoY growth rate in revenue?

**Q2. [Regional GM Request]**
Each regional general manager wants to know how their region is performing. Build a summary table of `region` showing: total transactions, total revenue, total profit, and average margin. Sort by total revenue descending. Which region is the top performer? Which is the weakest?

**Q3. [CFO — Profitability Audit]**
The CFO suspects some product categories are dragging down overall margins. Plot gross margin (%) by `category` using a horizontal barplot, sorted from highest to lowest. Identify any category with a margin below 25%. Are there categories where the company might be selling at a loss?

**Q4. [Operations — Quarterly Trend]**
The operations team wants a quarterly revenue trend chart. Group by `year` and `quarter`, sum `gross_revenue_php`, and plot as a multi-line chart (one line per year). Does Q4 consistently spike? What does this suggest about seasonality planning for inventory?

**Q5. [Supply Chain Manager]**
The supply chain manager is worried about stockouts. Calculate the stockout rate (`stockout_flag == 1`) per `category`. Plot as a barplot. Which category has the highest stockout risk? Cross-check with that category's total revenue — is it also a high-revenue category? If so, the risk is more urgent.

---

### Section 2 — Sales & Channel Analysis (Sales Director / Channel Manager)

**Q6. [Sales Director]**
The sales director wants to know which `sales_channel` drives the most revenue and which is most profitable. Build a grouped summary by `sales_channel` with: total revenue, total profit, average margin, and average order value (revenue ÷ transactions). Present as both a table and a side-by-side barplot.

**Q7. [Digital Team]**
The digital team is pitching for a larger budget. Filter to only online channels (`Website` and `Mobile App`). Compare their monthly revenue trends over the 3-year period using a line chart. Is one channel growing faster than the other?

**Q8. [Promotions Manager]**
The promotions team ran six different promo types. Compute the average gross margin by `promo_type`. Which promo type is least damaging to margins? Which one is the most costly? Should the business rethink its `Flash Sale` or `Buy1Get1` strategy based on what the data shows?

**Q9. [Payment Partnerships Team]**
The fintech partnerships team wants to know which payment methods are gaining traction. Plot the count and revenue share of each `payment_method` per year. Has GCash or Maya grown as a share of transactions from 2022 to 2024?

**Q10. [Store Format Analyst]**
Compare `store_format` performance across average transaction value, average margin, and return rate. Which format has the highest return rate? Discuss what operational action that might suggest.

---

### Section 3 — Customer Intelligence (CRM / Marketing Analytics)

**Q11. [CRM Manager — Segment Health]**
The CRM team tracks five customer segments. For each `customer_segment`, compute: transaction count, average revenue per transaction, average `customer_rating`, and return rate. Plot customer ratings by segment as a boxplot. Which segment is most valuable? Which one signals a retention problem?

**Q12. [Marketing Analyst — NPS Deep Dive]**
`nps_score` has 36% missing values. Before any analysis, assess whether the missingness seems random. Does NPS missing rate differ by `customer_segment` or `sales_channel`? Then for rows where NPS exists: classify scores as Detractor (0–6), Passive (7–8), or Promoter (9–10). Compute the Net Promoter Score = % Promoters − % Detractors. What is the overall NPS?

**Q13. [Retention Team]**
The retention team wants to understand the `At-Risk` and `Churned` segments better. Filter to these two segments only. Compare their distributions of `customer_rating`, `return_flag`, `discount_pct`, and `loyalty_points_earned` using side-by-side plots. What pattern emerges that might explain why they became At-Risk or Churned?

**Q14. [Loyalty Program Lead]**
The loyalty team wants to know if points earned correlate with customer retention. Plot `loyalty_points_earned` by `customer_segment` as a boxplot. Then compute the correlation between `loyalty_points_earned` and `gross_revenue_php`. Does earning more points drive more spending, or is it the reverse?

**Q15. [Product Manager — Returns Analysis]**
The product team wants to reduce return rates. Calculate return rate by `category` and by `return_reason`. Plot both as barplots. Which category has the most returns? Is "Defective" or "Changed Mind" the dominant reason? What does this suggest: a quality issue or a mismatch in customer expectations?

---

### Section 4 — Advanced / Senior Data Scientist Level

**Q16. [Sr. Data Scientist — Margin Decomposition]**
The analytics team wants to understand what drives gross margin variance. Build a correlation heatmap of numeric columns. Then run a focused analysis: does `discount_pct` explain margin drop linearly? Scatter plot `discount_pct` vs `gross_margin_pct` with a regression line. For every 10% increase in discount, roughly how much does margin fall?

**Q17. [Sr. Data Scientist — Revenue Forecasting Prep]**
To prepare data for a forecasting model, the data science team needs a clean monthly revenue time series. Aggregate total `gross_revenue_php` by `year` and `month`. Parse into a proper datetime index. Plot the full time series. Identify any anomalies — months with unusually low revenue. Are they real business dips or data quality issues?

**Q18. [Sr. Data Scientist — Customer Lifetime Value Proxy]**
A proper CLV model needs multiple transactions per customer. Filter to `customer_id` rows that are not null (exclude anonymous). Group by `customer_id` to compute: total revenue, total transactions, average order value, and average rating. Plot the distribution of total revenue per customer. What does the top 10% of customers contribute as a share of total revenue from identified customers?

**Q19. [Sr. Data Scientist — Supplier Profitability]**
The procurement team suspects some suppliers are systematically delivering lower-margin products. Group by `supplier` and compute: average `gross_margin_pct`, average `unit_cost_php`, total `gross_profit_php`, and average `customer_rating`. Plot average margin by supplier as a horizontal barplot. Flag any supplier where average margin is more than 1 standard deviation below the mean. These are candidates for renegotiation.

**Q20. [Sr. Data Scientist — Multivariate Basket Analysis Prep]**
Before building a recommendation model, the team wants to understand product co-purchase patterns at the category level. Filter to transactions where `quantity_sold >= 2` (bulk or bundle buys). Build a crosstab of `category` vs `promo_type` normalized by row. Plot as a heatmap. Which categories are most promo-dependent? Is there a category that sells in bulk even without promotions? What does that mean for its pricing power?

---

## Expected File Structure

```
retail_eda/
│
├── data/
│   └── retail_sales.csv
│
├── notebooks/
│   └── retail_eda.ipynb
│
├── outputs/
│   ├── q1_revenue_yoy.png
│   ├── q2_regional_summary.png
│   ├── q3_margin_by_category.png
│   ├── q4_quarterly_trend.png
│   ├── q5_stockout_by_category.png
│   ├── q6_channel_summary.png
│   ├── q7_digital_monthly_trend.png
│   ├── q8_margin_by_promo.png
│   ├── q9_payment_share_yoy.png
│   ├── q10_store_format_compare.png
│   ├── q11_segment_rating_boxplot.png
│   ├── q12_nps_analysis.png
│   ├── q13_atrisk_churned_compare.png
│   ├── q14_loyalty_by_segment.png
│   ├── q15_returns_analysis.png
│   ├── q16_margin_vs_discount.png
│   ├── q17_revenue_timeseries.png
│   ├── q18_clv_proxy_dist.png
│   ├── q19_supplier_margin.png
│   └── q20_category_promo_heatmap.png
│
└── README.md
```

---

## Notes on Intentional Data Quirks

These were built in deliberately — you're expected to find and handle them:

- `customer_id` is null for ~8% of rows (anonymous in-store walk-ins). Q18 requires you to filter these out properly.
- `nps_score` is missing ~36% of the time — not randomly. It's more likely to be missing for certain channels. Q12 asks you to verify this.
- `customer_rating` has ~7% nulls — roughly MCAR, but check.
- `delivery_days` and `shipping_cost_php` are 0 for in-store transactions by design, not error. Don't treat as nulls.
- `gross_margin_pct` can be negative on a few rows (deep discounts exceeding margin). These are real business events, not data errors.
- Q4 seasonality is baked in: Q4 volume is slightly inflated. Your trend chart in Q4 should surface this.
