# Banking Dataset Analysis — Telemarketing Campaign EDA

> **Project Stage:** Exploratory Data Analysis (EDA)
> **Internship:** Finlatics | February – May 2024
> **Author:** Kranti Prakash, IIT Jodhpur 

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Dataset Description](#dataset-description)
3. [Column Reference](#column-reference)
4. [Project Structure](#project-structure)
5. [Setup & Requirements](#setup--requirements)
6. [EDA — Questions Explored](#eda--questions-explored)
7. [Key Findings & Insights](#key-findings--insights)
8. [Correlation Analysis](#correlation-analysis)
9. [Limitations](#limitations)

---

## Project Overview

This project analyzes a **Portuguese bank's telemarketing campaign** data to understand what factors influence whether a client subscribes to a **term deposit**. The bank reached out to clients over phone calls, and the core business question is:

> **Which clients are most likely to subscribe — and how can the bank target them more efficiently?**

At this stage, the project focuses entirely on **Exploratory Data Analysis (EDA)** — understanding the data distribution, client demographics, campaign behavior, and identifying correlations with the subscription outcome.

---

## Dataset Description

| Property | Value |
|---|---|
| **File** | `banking_data.csv` |
| **Rows** | 45,216 |
| **Columns** | 19 |
| **Target Variable** | `y` (subscribed to term deposit: yes / no) |
| **Source** | Portuguese bank telemarketing campaign (based on the UCI Bank Marketing Dataset) |
| **Missing Values** | 9 total (3 each in `marital`, `marital_status`, `education`) — filled with `0` |

The dataset represents one record per client contact. Each row captures client demographics, their financial profile, details about how and when they were contacted, and whether they ultimately subscribed.

**Class Distribution (Target Variable `y`):**

| Outcome | Count | Percentage |
|---|---|---|
| No (did not subscribe) | 39,922 | 88.3% |
| Yes (subscribed) | 5,294 | 11.7% |

The dataset is **significantly imbalanced** — only about 1 in 9 clients subscribed.

---

## Column Reference

### Client Information

| Column | Type | Description | Notes |
|---|---|---|---|
| `age` | Integer | Client's age in years | Range: 18–95, Mean: ~40 |
| `job` | Categorical | Type of occupation | 12 categories (blue-collar, management, technician, etc.) |
| `marital` | Categorical | Marital status | married, single, divorced |
| `marital_status` | Categorical | Cleaned marital column | Same as `marital`; missing values filled with `0` |
| `education` | Categorical | Highest education level | primary, secondary, tertiary, unknown |
| `default` | Binary | Has credit in default? | yes / no |
| `balance` | Integer | Average yearly bank balance (euros) | Range: −8,019 to 102,127, Mean: 1,362 |
| `housing` | Binary | Has a housing loan? | yes / no |
| `loan` | Binary | Has a personal loan? | yes / no |

### Campaign Contact Details

| Column | Type | Description | Notes |
|---|---|---|---|
| `contact` | Categorical | Communication type used | cellular (64.8%), telephone (6.4%), unknown (28.8%) |
| `day` | Integer | Day of month of last contact | 1–31 |
| `month` | Categorical | Month of last contact | All 12 months; May is most frequent |
| `day_month` | Categorical | Combined day + month string | e.g., "5-May"; 318 unique values |
| `duration` | Integer | Duration of last call in seconds | Range: 0–4,918; **strongest predictor of subscription** |
| `campaign` | Integer | Number of contacts during this campaign | Range: 1–63 |

### Previous Campaign History

| Column | Type | Description | Notes |
|---|---|---|---|
| `pdays` | Integer | Days since client was last contacted in a prior campaign | −1 means never previously contacted |
| `previous` | Integer | Number of contacts before this campaign | Range: 0–275 |
| `poutcome` | Categorical | Outcome of the previous campaign | unknown (81.7%), failure (10.8%), success, other |

### Target

| Column | Type | Description |
|---|---|---|
| `y` | Binary | Did the client subscribe to a term deposit? (yes / no) |

---


## Setup & Requirements

The notebook was developed on **Google Colab**. To run it locally:

**Python Version:** 3.8+

**Required Libraries:**

```bash
pip install pandas numpy matplotlib seaborn scipy
```

| Library | Purpose |
|---|---|
| `pandas` | Data loading, cleaning, groupby aggregations |
| `numpy` | Numerical operations |
| `matplotlib` | Bar charts, histograms, pie charts |
| `seaborn` | Styled charts, correlation heatmap |
| `scipy.stats` | Normal distribution fitting |


---

## EDA — Questions Explored

The notebook is structured as **18 business questions**, each answered with a chart and summary statistics.

| # | Question | Chart Type |
|---|---|---|
| 1 | What is the age distribution of clients? | Histogram + normal curve |
| 2 | How does job type vary across clients? | Horizontal bar chart |
| 3 | What is the marital status distribution? | Bar chart + pie chart |
| 4 | What are the education levels of clients? | Bar chart + pie chart |
| 5 | What proportion of clients have credit in default? | Bar chart + pie chart |
| 6 | What is the distribution of average yearly bank balance? | Histogram + describe() |
| 7 | How many clients have a housing loan? | Bar chart + pie chart |
| 8 | How many clients have a personal loan? | Bar chart |
| 9 | What communication types were used in the campaign? | Bar chart + pie chart |
| 10 | How does last contact month vary? | Horizontal bar chart |
| 11 | What is the distribution of last contact day? | Bar chart |
| 12 | What is the average call duration by job category? | Bar chart + pie chart |
| 13 | How many contacts were made per job type? | Bar chart |
| 14 | How many days passed since the last previous campaign contact? | Histogram (pdays > 0 only) |
| 15 | How many contacts were made before the current campaign? | Histogram |
| 16 | What were the outcomes of previous marketing campaigns? | Bar chart + pie chart |
| 17 | What is the subscription distribution (target variable)? | Bar chart + pie chart |
| 18 | Are there numeric correlations with subscription likelihood? | Seaborn heatmap |

---

## Key Findings & Insights

### Client Demographics
- The **largest job group** is blue-collar workers (9,732), closely followed by management (9,460) and technicians (7,597).
- **60.2% of clients are married**, 28.3% single, 11.5% divorced.
- **51.3% have secondary education**, 29.4% tertiary — the campaign targets a broad middle-income demographic.
- **98.2% have no credit default**, showing the client base is financially stable overall.

### Financial Profile
- Average yearly bank balance: **€1,362** (std: €3,044), with balances ranging from −€8,019 to +€102,127.
- **55.6% have a housing loan**; only **16.1% have a personal loan**.

### Campaign Behavior
- **May is the most active month** — 13,766 clients were contacted, nearly double July (6,895).
- **Cellular was the dominant contact method** (64.8%), with a surprisingly high 28.8% listed as "unknown."
- **81.7% of clients had no prior campaign history** (`poutcome = unknown`), meaning this was their first interaction.
- Among the 8,260 previously contacted clients, the average gap since last contact was **224.5 days (~7.5 months)**.

### What Drives Subscription
- **Longer calls → higher subscription** (duration r = +0.394): Engaged conversations strongly correlate with conversion.
- **Previous campaign contact helps** (pdays r = +0.104, previous r = +0.094): Familiarity from prior interactions improves outcomes.
- **More calls in this campaign hurt** (campaign r = −0.073): Over-contacting a client reduces their likelihood of subscribing. Quality over quantity.
- **Balance has a small positive effect** (r = +0.053): Slightly wealthier clients are marginally more likely to subscribe.

---

## Correlation Analysis

**`r` (Pearson correlation coefficient)** measures how strongly a numeric feature is linearly related to the subscription outcome (`y`). A positive `r` means clients with higher values of that feature are more likely to subscribe; a negative `r` means higher values are associated with lower subscription likelihood. The closer `r` is to ±1, the stronger the relationship — values near 0 indicate little to no linear influence on whether a client subscribes.

Pearson correlations between numeric features and the target variable `y` (encoded as 1=yes, 0=no):

| Feature | Correlation with `y` | Direction |
|---|---|---|
| `duration` | **+0.394** | Longer calls → more likely to subscribe |
| `pdays` | +0.104 | More recent prior contact → higher chance |
| `previous` | +0.094 | More prior contacts → higher chance |
| `balance` | +0.053 | Higher balance → slightly more likely |
| `age` | +0.026 | Negligible |
| `day` | −0.028 | Negligible |
| `campaign` | **−0.073** | More calls this campaign → less likely |

> **Important note on `duration`:** Call duration is only known *after* the call ends. It cannot be used as an input feature in a real-time prediction system for *deciding who to call*. It is analytically significant but not actionable pre-call.

---

## Limitations

### Current Limitations
- `duration` is the strongest predictor but is a **post-call feature** — unavailable before the call is made.
- The `poutcome` and `contact` columns have high "unknown" rates (81.7% and 28.8% respectively), limiting their predictive power without imputation strategies.
- No statistical significance testing was performed on the distributions.

<!-- ### Next Stage — Predictive Modeling
The EDA lays a strong foundation for building a classification model. Planned next steps:

1. **Feature Engineering**
   - Encode categorical variables (one-hot or label encoding)
   - Create a binary flag for `pdays` (was/was not previously contacted)
   - Scale numerical features

2. **Handle Class Imbalance**
   - Apply SMOTE or use `class_weight='balanced'` in model training

3. **Model Training**
   - Baseline: Logistic Regression
   - Primary: Random Forest, XGBoost / LightGBM

4. **Evaluation**
   - Focus on AUC-ROC, Precision, Recall, and F1 Score (not raw accuracy)
   - Generate a ranked probability list for the marketing team

5. **Business Output**
   - Produce a **prioritized call list** — rank clients by subscription probability so the bank's agents call the most likely subscribers first, reducing cost and improving conversion rate.

--- -->

*This project is part of a data science internship at Finlatics. Dataset based on the UCI Bank Marketing Dataset — Moro et al., 2014.*
