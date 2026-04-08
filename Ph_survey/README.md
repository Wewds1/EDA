

## Philippine Household Survey: Socioeconomic EDA Pipeline

This repository hosts a comprehensive Exploratory Data Analysis (EDA) workflow focused on Philippine household dynamics. Using a simulated dataset (2018–2023), the project simulates a real-world data science environment by incorporating intentional data quality issues, requiring both programmatic cleaning and statistical interpretation.

## Project Objectives
The primary goal is to extract actionable socioeconomic insights while maintaining a high standard of data hygiene. This project demonstrates proficiency in:

Data Auditing: Identifying systemic missingness and placeholder artifacts.

Feature Engineering & Cleaning: Rectifying invalid income entries and preparing variables for longitudinal analysis.

Statistical Visualization: Using multi-variate plots to uncover regional disparities.

Policy-Relevant Insights: Investigating the "Digital Divide" through the lens of educational attainment.

## Repository Architecture
Plaintext
├── data/
│   └── ph_household_survey.csv   # 1,200 samples across 26 socioeconomic features
├── notebooks/
│   └── main.ipynb               # End-to-end analysis (Cleaning to Correlation)
├── output/                      # Exported visualizations (Seaborn/Matplotlib)
│   ├── q3_distribution.png
│   ├── q20_digital_divide.png
│   └── ...
├── eda_problems.md              # Guided task list and business logic
└── README.md                    # Project documentation
## Analytical Deep Dive
The analysis is structured into four distinct phases:

1. Data Quality & Pre-processing
Before analysis, the dataset undergoes a rigorous quality check. This includes:

Missing Value Profiling: Visualizing null density across the 26 columns.

Outlier & Artifact Detection: Identifying "dirty" values in the income column (e.g., negative values or improbable placeholders) and applying logic-based imputation or removal.

2. Univariate & Bivariate Analysis
We explore the distribution of key metrics such as household size, regional density, and health indicators.

Tools Used: KDE plots for income distribution, boxplots for regional spending variances, and violin plots to observe density and range simultaneously.

3. Correlation & Multivariate Relationships
We calculate a Pearson correlation matrix to identify strong predictors of household welfare.

Highlight: Extracting the "Top N" relationships to focus on high-impact variables (e.g., the link between education level and household assets).

4. The Digital Divide Analysis
The final stage focuses on a critical social metric: Internet Access. By segmenting the population by education level, the analysis quantifies the gap in digital connectivity, providing a data-driven look at information equity in the Philippines.

## Tech Stack
Language: Python 3.x

Data Manipulation: pandas, numpy

Visualization: matplotlib, seaborn

Environment: Jupyter Notebook / VS Code

## Usage Instructions
Clone the repository:

Bash
pip install pandas numpy matplotlib seaborn
Execute Analysis:
Open main.ipynb and run all cells. The script will automatically generate and save 17+ visualizations to the output/ directory.

## Key Findings & Outputs
The analysis generates a suite of figures (Q3–Q20) that visualize:

Income Skewness: How wealth is concentrated across different regions.

Education vs. Connectivity: A heatmap/barplot showing the probability of internet access based on the head of household's education.

Socioeconomic Clusters: Grouped comparisons that highlight the intersection of health and wealth.

