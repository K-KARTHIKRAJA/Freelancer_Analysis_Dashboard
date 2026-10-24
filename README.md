# Freelancer Analysis Dashboard
> Predicting Freelance Job Success Using Machine Learning

---

## 📌 Project Overview

This project investigates whether machine learning models can predict the success of a freelance job using structured metadata available at the time of job posting and post-hiring performance metrics. The project was carried out as part of a Data Analytics Internship.

The analysis is divided into two prediction models:
- **Model 1 (Pre-Hiring)** — Can we predict job success before hiring using only job posting data?
- **Model 2 (Post-Hiring)** — Given a freelancer is hired, can we predict successful completion?

A key theme throughout this project is **analytical rigor** — three separate data leakage instances were identified and corrected, and negative results are reported honestly with business recommendations.

---

## 🗂️ Dataset

- **Source:** Kaggle
- **Records:** 1,000 freelance job postings
- **Columns:** 12
- **Target Variable:** `success` (0 = Failed, 1 = Succeeded)

| Column | Description |
|---|---|
| job_id | Unique job identifier |
| category | Job category (7 types) |
| posted_date | Date job was posted |
| budget_usd | Job budget in USD |
| duration_days | Expected duration in days |
| num_applicants | Number of applicants |
| hired | Whether a freelancer was hired (0/1) |
| freelancer_rating | Rating of hired freelancer |
| completion_time_days | Actual days to complete |
| success | Final outcome (0/1) |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Python | Data cleaning, EDA, modelling |
| Pandas & NumPy | Data manipulation |
| Matplotlib & Seaborn | Visualisation |
| Scikit-learn | Machine learning models |
| Google Colab | Development environment |
| Google Drive | Dataset storage |
| Power BI | Interactive dashboard |
| Excel | Initial data exploration |

---

## 🔄 Project Workflow

```
Raw Dataset (Kaggle CSV)
        ↓
Data Inspection (Excel + Python)
        ↓
Data Quality Checking & Cleaning
        ↓
Exploratory Data Analysis (EDA)
        ↓
Feature Engineering
        ↓
Data Leakage Detection & Correction
        ↓
Machine Learning Model Development
        ↓
Model Evaluation & Comparison
        ↓
Power BI Dashboard
        ↓
Business Insights & Reporting
```

---

## ⚙️ Feature Engineering

Six new features were created from existing columns:

| Feature | Logic | Meaning |
|---|---|---|
| competition_level | Cut num_applicants into Low/Medium/High | Competition intensity |
| budget_category | Cut budget_usd into Small/Mid/High | Financial tier |
| efficiency_ratio | completion_time_days / duration_days | On-time delivery measure |
| is_top_rated | 1 if rating >= 4.5, else 0 | Premium freelancer flag |
| posted_month | Extracted from posted_date | Seasonal pattern |
| posted_dayofweek | Extracted from posted_date | Day-of-week pattern |

---

## 🚨 Data Leakage — 3 Instances Detected & Corrected

This was the most critical part of the project. Three separate leakage instances were found and fixed:

**Leakage 1 — `hired` column**
Every `hired = 0` row had `success = 0` without exception. The model would just read the answer rather than learn real patterns.
> ✅ Fix: Removed `hired` from all feature sets

**Leakage 2 — Post-completion features in pre-hiring model**
`completion_time_days` and `efficiency_ratio` are only known after a job ends — not at posting time.
> ✅ Fix: Removed both from Model 1 feature set only

**Leakage 3 — Missing value imputation leakage**
Filling `freelancer_rating` with median (4.2) gave all 293 non-hired rows an identical value. The model detected this pattern and achieved a false accuracy of **90.5%**.
> ✅ Fix: Removed `freelancer_rating` and `is_top_rated` from Model 1 entirely

---

## 🤖 Machine Learning Models

### Model 1 — Pre-Hiring Prediction
**Features used:** 17 (budget, duration, applicants, category, date features, competition level, budget category)

| Algorithm | Accuracy | Baseline |
|---|---|---|
| Logistic Regression | 60.5% | 61.0% |
| Decision Tree | 60.0% | 61.0% |
| Random Forest | 60.0% | 61.0% |

### Model 2 — Post-Hiring Prediction
**Dataset:** Filtered to hired = 1 only (707 rows)

| Algorithm | Accuracy | Baseline |
|---|---|---|
| Logistic Regression | 86.6% | 86.6% |
| Decision Tree | 83.1% | 86.6% |
| Random Forest | 86.6% | 86.6% |

**Key Notes:**
- `stratify=y` used in train-test split (80:20)
- `StandardScaler` applied for Logistic Regression only
- `class_weight='balanced'` used to handle class imbalance
- Confusion matrix confirmed model never correctly predicted a single failure case

---

## 📊 Key Findings

**Finding 1 — Pre-hiring prediction is not feasible**
All models matched the 61% majority class baseline. Budget and applicant count showed near-zero correlation with success.

**Finding 2 — Post-hiring prediction also shows no meaningful lift**
All models matched the 86.6% baseline. Confusion matrix confirmed the model predicted success for every single test case.

**Finding 3 — Success and failure profiles are nearly identical**

| Feature | Failed Jobs | Successful Jobs |
|---|---|---|
| budget_usd | $493.6 | $505.2 |
| duration_days | 9.0 | 9.3 |
| num_applicants | 6.8 | 6.8 |
| freelancer_rating | 4.12 | 4.18 |
| completion_time_days | 9.16 | 9.64 |

**Finding 4 — Category shows the widest variation**
Data Science leads at 64.4% success rate. Marketing trails at 54.5%.

---

## 💼 Business Recommendation

Freelance platforms should collect behavioural and process-related data to build a reliable predictor:
- Client-freelancer communication quality
- Scope change frequency
- Dispute and revision history
- Milestone completion tracking
- Client satisfaction at intermediate stages

---

## 📁 Project Structure

```
freelancer-income-volatility-analyzer/
│
├── data/
│   ├── freelancer_data.csv              # Raw dataset
│   ├── freelancer_cleaned.csv           # Cleaned dataset
│   ├── final_dashboard_data.csv         # Final dataset with predictions
│   ├── model_comparison.csv             # Model accuracy results
│   ├── model1_feature_importance.csv    # Model 1 feature importance
│   └── model2_feature_importance.csv    # Model 2 feature importance
│
├── notebooks/
│   └── analysis.ipynb                   # Full Colab notebook
│
├── dashboard/
│   └── freelancer_dashboard.pbix        # Power BI dashboard file
│
└── README.md
```

---

## 📈 Power BI Dashboard

3-page interactive dashboard built in Power BI Desktop:

**Page 1 — Overview**
KPI cards, success rate by category, hired vs not-hired donut chart, month slicer, navigation buttons

**Page 2 — Pre & Post Hiring Analysis**
Pre-hiring feature comparisons, post-hiring metric comparisons, model accuracy vs baseline cards, feature importance charts

**Page 3 — Model Comparison & Conclusion**
6-bar accuracy comparison chart, key finding statement, business recommendation, skill tags

---

## 🧠 Skills Demonstrated

- `Data Leakage Detection` — 3 instances found and corrected
- `Class Imbalance Handling` — class weights, baseline comparison
- `Feature Engineering` — 6 new features created
- `Honest Statistical Reporting` — negative results reported clearly
- `End-to-End ML Pipeline` — from raw CSV to Power BI dashboard
- `Business Storytelling` — findings converted to actionable recommendations

---

## 📚 References

- Scikit-learn Documentation — https://scikit-learn.org
- Pandas Documentation — https://pandas.pydata.org/docs
- Microsoft Power BI Documentation — https://docs.microsoft.com/power-bi
- Kaggle Dataset — https://www.kaggle.com *(insert exact URL)*
- Breiman, L. (2001). Random Forests. Machine Learning, 45(1), 5–32

---

## 👤 Author

**Karthikraja K**
B.Tech — Computer Science and Engineering

Kalasalingam Academy of Research and Education

Data Analytics Intern @ Dot Com Infoway Private Limited

---

> ⭐ If you found this project useful, consider giving it a star!
