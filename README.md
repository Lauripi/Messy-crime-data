4. Crime Incidents - Data Cleaning, Exploration and Statistical Analysis - Python

A Python workflow applied to a messy, simulated crime dataset: from raw data with realistic quality problems to an analysis-ready table, followed by exploratory analysis and statistical tests. Cleaning decisions are checked and explained, and imputed values are flagged rather than hidden.

## Overview

- **Dataset:** simulated crime incident dataset from [Kaggle](https://www.kaggle.com/datasets/sananshaikh/messy-crime-dataset-for-data-cleaning-practice), 5,250 records describing crimes, locations, victims, suspects, officers, arrests, property loss and case status
- **Task:** clean the raw data, then use it to answer three analysis questions
- **Focus:** transparent cleaning decisions, checks before and after each transformation, and statistical tests chosen to fit the data

## Cleaning at a glance

| Problem | Action | Result |
|---|---|---|
| Duplicate records | Removed | 5,250 → 5,050 rows |
| Typos, abbreviations and synonyms | Similarity detection (`difflib`) + manual review | e.g. crime type: 58 spellings → 4 categories; district: 16 → 10 |
| Malformed amounts (e.g. `35446.9.0`) | Repaired before numeric conversion | 136 values recovered |
| Three date formats, incl. US month-first | Each format converted explicitly | No swapped days and months |
| Impossible values (negative numbers, ages up to 298, latitudes > 90) | Sign corrected or set to missing | 331 ages and 176 coordinates set to missing |
| Missing victim/suspect gender | Inferred from first names (`gender-guesser`) | 1,964 values imputed and flagged |

## Results at a glance

| Question | Test | Result |
|---|---|---|
| Do incident counts differ between cities? | Chi-square goodness-of-fit | Yes (χ² = 15.6, p = 0.03); Riverside has the most incidents (705) |
| Does the crime-type mix differ between cities? | Chi-square test of independence | No (p = 0.26) |
| Did incidents change over time (2018–2024)? | Poisson regression on monthly counts | No significant trend (+1.1% per year, p = 0.12) |
| Do incidents vary by season? | Chi-square goodness-of-fit | No (p = 0.32) |
| Is crime severity linked to property loss? | Kruskal–Wallis, Spearman correlation | No (p = 0.79; ρ = 0.01, p = 0.39) |

## Approach

1. **Initial data audit:** inspected datatypes, missing values, duplicates, categorical levels and numeric ranges to identify the main data-quality problems.\
   → *Data quality assessment, duplicate detection*
2. **Text and datatype standardization:** trimmed whitespace, standardized capitalization, stored identifiers as text and converted wrongly stored numbers to numeric values.\
   → *String normalization, datatype conversion*
3. **Categorical cleaning:** detected spelling errors by string similarity, reviewed every suggested correction manually, harmonized abbreviations and synonyms, and grouped crime types into four broad categories.\
   → *Typo detection, synonym harmonization, manual quality control*
4. **Missing-data handling:** assessed missingness variable by variable instead of applying one imputation rule; imputed gender from first names with explicit imputation and uncertainty flags, and left other missing values as missing.\
   → *Selective imputation with explicit flags*
5. **Numeric and geographic validation:** checked distributions and plausible ranges, corrected sign errors and set impossible values to missing.\
   → *Range validation*
6. **Date and time preprocessing:** split date and time, standardized three date formats, and created time-of-day and season variables.\
   → *Mixed date formats, feature engineering*
7. **Exploration and analysis:** described the main variables (summary statistics, bar charts, histograms, box plots, trend plots), then answered three questions with statistical tests. Numeric variables are not normally distributed, so non-parametric tests were used where needed.\
   → *Chi-square tests, Poisson regression for count data, non-parametric tests (Kruskal–Wallis, Spearman), matplotlib*

## Tech stack

Python · pandas · NumPy · SciPy · statsmodels · matplotlib · difflib · gender-guesser

## Files

- `4. Crimes_cleaning_data.ipynb` — full analysis notebook
