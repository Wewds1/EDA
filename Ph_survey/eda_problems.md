# EDA Problems: Philippine Household Survey Dataset

**Dataset:** `ph_household_survey.csv`  
**Rows:** 1,200 | **Columns:** 26  
**Context:** Simulated household survey covering socioeconomic indicators across Philippine regions (2018–2023). Not official PSHS/PSA data — built for EDA practice.

---

## Dataset Overview

| Column | Type | Notes |
|---|---|---|
| `respondent_id` | str | Unique ID |
| `survey_year` | int | 2018–2023 |
| `region` | str | 17 regions |
| `province` | str | Subset of provinces |
| `sex` | str | Male / Female |
| `age` | int | 15–85 |
| `civil_status` | str | Single, Married, etc. |
| `education_level` | str | Ordinal, 6 levels |
| `years_of_schooling` | int | Continuous proxy for education |
| `employment_status` | str | 6 categories |
| `occupation_type` | str | 10 sectors |
| `monthly_income_php` | float | **Has nulls + a dirty value (-999)** |
| `household_size` | int | Number of people in household |
| `num_children` | int | Children under the household |
| `housing_type` | str | 5 types |
| `water_source` | str | 6 types |
| `toilet_facility` | str | 4 types |
| `electricity_source` | str | 4 types |
| `internet_access` | str | 4 types |
| `health_insurance` | str | 4 types |
| `food_insecurity_score` | int | 1–5 scale (5 = most insecure) |
| `bmi` | float | **Has nulls** |
| `mental_health_score` | float | 0–27 PHQ-like scale; **has nulls** |
| `dist_to_health_facility_km` | float | Distance to nearest clinic/hospital |
| `received_gov_aid` | int | 0 or 1 |
| `gov_aid_amount_php` | int | 0 if no aid received |

---

## Problems

### Section 1 — Data Quality and First Inspection

**Q1.** Load the dataset and run a full first-pass inspection: shape, dtypes, and null counts per column. Which three columns have missing values, and what percentage of each is missing?

**Q2.** `monthly_income_php` contains a dirty value that should not be there. Find it, explain why it's invalid, and decide how to handle it (drop the row, replace with NaN, or cap it). Write the code for your chosen approach.

**Q3.** Check `education_level` and `employment_status` for unexpected or misspelled categories. Print their full value counts. Are there any categories that look wrong?

**Q4.** `age` should realistically be between 15 and 85 for a household survey of working-age adults. Verify this. Are there any out-of-range values? Plot a histogram of `age`.

**Q5.** Assess the missingness in `monthly_income_php`. Is it likely MCAR, MAR, or MNAR? Cross-tabulate null vs. non-null income against `employment_status` to support your answer.

---

### Section 2 — Univariate Analysis

**Q6.** Plot the distribution of `monthly_income_php` (after cleaning dirty values). The distribution is likely skewed — apply a log transformation and plot again. Describe what changes and why log-transforming income is common in socioeconomic analysis.

**Q7.** Plot a countplot of `region`. Which region has the most respondents? Is this distribution roughly what you'd expect for a national survey, or does it look imbalanced?

**Q8.** Plot the distribution of `mental_health_score`. Does it look roughly normal? Note the range and what a higher score means in this context. Overlay a KDE curve.

**Q9.** Plot a countplot of `education_level` ordered from lowest to highest level (not alphabetically). What is the most common education level in the dataset?

**Q10.** `food_insecurity_score` runs from 1 to 5. Plot its distribution as both a countplot and a boxplot. What does the median tell you about the sample?

---

### Section 3 — Bivariate and Grouped Analysis

**Q11.** Plot a boxplot of `monthly_income_php` grouped by `education_level` (ordered lowest to highest). Does income increase with education level? Are there outliers in any group? Use log-scale on the y-axis.

**Q12.** Using a grouped barplot or pointplot, show the average `food_insecurity_score` by `region`. Which region shows the highest average food insecurity? Does this align with what you know about regional poverty in the Philippines?

**Q13.** Compare `mental_health_score` distributions between `survey_year` 2019 and 2021 using side-by-side boxplots or violin plots. Do you see a difference? What might explain it?

**Q14.** Create a crosstab (pivot table) of `housing_type` vs `water_source`. Normalize by row so you can see the proportion of water source types within each housing category. Display as a heatmap using seaborn.

**Q15.** Filter the dataset to only respondents who `received_gov_aid == 1`. Among this group, plot `monthly_income_php` vs `food_insecurity_score` as a scatterplot. Add a regression line. Is there a visible trend?

---

### Section 4 — Multi-variable and Correlation Analysis

**Q16.** Build a correlation matrix for all numeric columns. Plot it as a heatmap. Identify the top 3 strongest positive and top 3 strongest negative correlations. Do any of them surprise you?

**Q17.** Create a new column `income_per_capita` = `monthly_income_php` / `household_size`. Then plot its distribution by `region` using a boxplot. Which regions show the widest spread in per-capita income?

**Q18.** Group by `region` and compute the mean values of `food_insecurity_score`, `dist_to_health_facility_km`, and `mental_health_score`. Display the result as a styled DataFrame (you can use `.style.background_gradient()`). What regional pattern do you notice?

**Q19.** Plot a pairplot of `monthly_income_php` (log-transformed), `years_of_schooling`, `food_insecurity_score`, and `mental_health_score`, colored by `sex`. Do male and female respondents cluster differently on any of these dimensions?

**Q20.** Filter the dataset to respondents aged 18–60 who are `Employed` or `Self-Employed`. Within this subset, compare `internet_access` distribution across `education_level` groups using a normalized stacked bar chart. What does this suggest about the digital divide?

---

## Expected File Structure

When you submit your work, organize it like this:

```
eda_ph_survey/
│
├── data/
│   └── ph_household_survey.csv
│
├── notebooks/
│   └── eda_ph_survey.ipynb        # your main notebook
│
├── outputs/
│   ├── q4_age_histogram.png
│   ├── q6_income_dist_raw.png
│   ├── q6_income_dist_log.png
│   ├── q7_region_countplot.png
│   ├── q8_mh_score_kde.png
│   ├── q9_education_countplot.png
│   ├── q10_food_insecurity.png
│   ├── q11_income_by_education.png
│   ├── q12_food_insecurity_by_region.png
│   ├── q13_mh_2019_vs_2021.png
│   ├── q14_housing_water_heatmap.png
│   ├── q15_income_vs_food_scatter.png
│   ├── q16_correlation_heatmap.png
│   ├── q17_income_percapita_region.png
│   ├── q18_regional_summary.png
│   ├── q19_pairplot.png
│   └── q20_internet_by_education.png
│
└── README.md                      # optional but good practice
```

Your notebook should have one clearly labeled cell per question — either a markdown cell with the question number, or a comment at the top of the code cell. This makes it easy to review.

---

## Notes on Intentional Data Issues

The dataset has a few deliberate problems baked in. You're expected to find and handle them:

- `monthly_income_php` has **nulls** (MNAR — unemployed respondents are more likely to skip it) and at least **one dirty numeric value**
- `bmi` and `mental_health_score` have **random nulls** (~10–12%)
- `mental_health_score` was artificially inflated for **2020 and 2021** to reflect pandemic-era stress — your job in Q13 is to surface this
- `province` only covers a subset of regions, so many rows will share the same province label

These are real-world data quality patterns. How you handle them is part of the exercise.
