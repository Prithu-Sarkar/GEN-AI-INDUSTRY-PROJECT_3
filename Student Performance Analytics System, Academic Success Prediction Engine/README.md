# 🎓 Student Performance Intelligence Platform
### *Advanced Edition — Ultra-Granular EDA · Statistics · Feature Engineering · ML · Explainable AI · Fairness Audit*

---

> **A production-grade, end-to-end data science pipeline built on the UCI-style `StudentsPerformance.csv` dataset (1,000 students) — spanning 16 rigorous phases from raw data to a fairness-audited, explainable champion model.**

---

## 📌 At a Glance

| | |
|---|---|
| 🗂️ **Dataset** | 1,000 students · 8 raw columns · 0 missing values |
| 📊 **Visualizations Generated** | **85** saved PNG charts |
| 📁 **CSV Artifacts** | **39** structured tables |
| 🧠 **Models Trained** | 10 regression models (5 baseline + 4 advanced + 1 mean-baseline) + 1 classification companion |
| ⚙️ **Hyperparameter Tuning** | Top-3 tunable models via `GridSearchCV` + 5-fold CV |
| 🔍 **Explainability** | SHAP (TreeExplainer) **and** LIME, cross-validated against each other |
| ⚖️ **Fairness Audit** | Four-fifths (80%) disparate impact rule across gender, race/ethnicity, lunch |
| 📦 **Final Package** | 1 executed `.ipynb` + 1 validated `.zip` (124 files) |
| ✅ **Sanity Checks** | Explicit `assert`-based checkpoint after **every phase** |

---

## 🗺️ Table of Contents

- [Project Structure](#-project-structure)
- [The 16 Phases](#-the-16-phases)
- [Key Findings](#-key-findings)
- [Champion Model](#-champion-model)
- [Explainability Highlights](#-explainability-highlights)
- [Fairness & Bias Audit](#️-fairness--bias-audit)
- [How to Run](#-how-to-run)
- [Tech Stack](#-tech-stack)
- [Notebook Integrity Guarantees](#-notebook-integrity-guarantees)
- [Deliverables](#-deliverables)

---

## 📁 Project Structure

```
Student_Performance_Intelligence_Platform/
│
├── Student_Performance_Intelligence_Platform_ADVANCED.ipynb   ← Main executed notebook
├── Student_Performance_Intelligence_Platform_Artifacts.zip    ← All 85 visuals + 39 CSVs, zipped
├── README.md                                                  ← You are here
│
└── outputs/  (inside the zip)
    ├── phase02_audit/         → data quality, outliers, demographic representation
    ├── phase03_eda/           → 37 ultra-granular EDA visuals + summary CSVs
    ├── phase04_stats/         → hypothesis tests, effect sizes, CI plots
    ├── phase05_features/      → engineered dataset, feature dictionary
    ├── phase06_selection/     → mutual information, F-scores, VIF report
    ├── phase07_baseline/      → 5 baseline model results
    ├── phase08_advanced/      → 4 advanced model results + comparisons
    ├── phase09_tuning/        → GridSearchCV results, before/after tuning
    ├── phase10_comparison/    → champion selection, residuals, learning curve
    ├── phase11_shap/          → SHAP beeswarm, bar, dependence, waterfall plots
    ├── phase12_lime/          → LIME local explanations (3 instances)
    ├── phase13_importance/    → coefficients, RF/GB importance, permutation importance
    ├── phase14_fairness/      → group bias metrics, disparate impact ratios
    ├── phase15_errors/        → error buckets, confusion matrix, ROC/AUC
    └── phase16_report/        → executive summary, artifact inventory, dashboard
```

---

## 🧭 The 16 Phases

| # | Phase | What Happens |
|---|-------|--------------|
| 1 | **Setup & Load** | Reproducible environment, seeded RNG, snake_case column normalization |
| 2 | **Data Audit** | Missingness, dtypes, IQR outlier detection, demographic representation |
| 3 | **Ultra-Granular EDA** | Distributions (hist/box/violin/QQ) × 3 scores, 5 category count-plots, 15 category-vs-score boxplots, correlation heatmap, pairplot, interaction plots |
| 4 | **Advanced Statistics** | Shapiro-Wilk normality, Welch's t-test, Mann-Whitney U, Levene's test, ANOVA, Kruskal-Wallis, Cohen's *d*, eta-squared |
| 5 | **Feature Engineering** | 17 new features: composite scores, ordinal parental education, pass/fail target, 5-tier performance category, one-hot race encoding |
| 6 | **Feature Selection** | F-regression/F-classif, mutual information, VIF multicollinearity check |
| 7–8 | **Modeling Framework** | 5 baseline + 4 advanced models, 80/20 split **+** 5-fold CV, R²/RMSE/MAE |
| 9 | **Hyperparameter Tuning** | Top-3 tunable models optimized via `GridSearchCV` |
| 10 | **Final Comparison** | Champion selection, predicted-vs-actual, residuals, learning curve |
| 11 | **SHAP Explainability** | Beeswarm summary, mean-\|SHAP\| bar, dependence plots, waterfall plot |
| 12 | **LIME Explainability** | Local surrogate explanations for 3 representative test instances |
| 13 | **Cross-Method Importance** | Linear coefficients, RF/GB importance, permutation importance, ranking comparison |
| 14 | **Fairness & Bias** | Group-wise MAE/RMSE bias check + four-fifths disparate impact rule |
| 15 | **Error Analysis** | Absolute-error buckets, worst-10 predictions, confusion matrix, ROC/AUC |
| 16 | **Packaging** | Executive summary, artifact inventory, dashboard figure, validated ZIP |

---

## 🔑 Key Findings

- **Test preparation completion, standard lunch, and parental education level** are the most consistent predictors of academic performance — confirmed independently by SHAP, permutation importance, and linear coefficients.
- **Demographic/administrative features alone explain a modest share of score variance** (champion test R² ≈ 0.18) — this is an honest, realistic finding: academic scores are driven overwhelmingly by factors *not present in this dataset* (student effort, instruction quality, prior knowledge), and the notebook does not overstate predictive power.
- **Tree ensembles (Random Forest, Extra Trees) overfit** on this small categorical feature space — regularized linear models (Ridge) generalize best, a valuable lesson in *matching model complexity to data complexity*.
- **Reading and writing scores correlate very strongly** with each other, and moderately with math — consistent with shared underlying "literacy" ability.

---

## 🏆 Champion Model

| Metric | Value |
|---|---|
| **Model** | Ridge Regression (tuned) |
| **Best `alpha`** | 10.0 |
| **Test R²** | 0.183 |
| **Test RMSE** | 14.10 |
| **Test MAE** | 11.17 |

*Selected from 10 baseline/advanced models + 3 hyperparameter-tuned finalists, based on held-out test R² after cross-validation.*

**Classification companion (pass/fail, threshold = 60):** Logistic Regression → **ROC-AUC ≈ 0.68**

---

## 🔬 Explainability Highlights

| Method | Top Driver Identified |
|---|---|
| **SHAP (mean \|value\|)** | `has_standard_lunch` |
| **Permutation Importance** | Consistent with SHAP ranking |
| **LIME (local, 3 instances)** | Same top features surfaced at the individual-prediction level |

Two *independent* explainability frameworks (game-theoretic SHAP vs. local-surrogate LIME) converge on the same top drivers — strong evidence the signal is genuine, not a modeling artifact.

---

## ⚖️ Fairness & Bias Audit

Every prediction from the champion model was audited against the **four-fifths (80%) rule** across:

- **Gender** (female / male)
- **Race/ethnicity** (groups A–E)
- **Lunch type** (standard / free-reduced)

Group-wise MAE, RMSE, and mean actual-vs-predicted gaps are logged in `phase14_fairness/fairness_bias_group_metrics.csv`, with any group falling below the 80% threshold explicitly flagged in `disparate_impact_four_fifths_rule.csv` — because a model that predicts well *on average* can still fail specific groups, and that has to be checked, not assumed away.

---

## 🚀 How to Run

1. **Open** `Student_Performance_Intelligence_Platform_ADVANCED.ipynb` in Jupyter Lab, Jupyter Notebook, VS Code, or Google Colab.
2. The notebook is **already executed** — all 85 visuals and every table are embedded inline, so you can read it top-to-bottom with zero setup.
3. To **re-run from scratch**: `Kernel → Restart & Run All`. Ensure `StudentsPerformance.csv` is in the same working directory, and install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy scikit-learn statsmodels shap lime
   ```
4. Unzip `Student_Performance_Intelligence_Platform_Artifacts.zip` to access every visual and CSV as a standalone file (useful for slide decks, reports, or feeding into a document-generation pipeline).

---

## 🧰 Tech Stack

`pandas` · `numpy` · `matplotlib` · `seaborn` · `scipy` · `scikit-learn` · `statsmodels` (VIF) · `shap` · `lime`

---

## 🛡️ Notebook Integrity Guarantees

This notebook was specifically built and validated to avoid the common causes of **"invalid notebook" errors on GitHub / GitLab / nbviewer**:

- ✅ Written directly via the `nbformat` Python API (no hand-crafted JSON strings, no quote-escaping bugs)
- ✅ `nbformat.validate()` passes with **zero errors**
- ✅ Valid `nbformat_minor = 5` with a **unique `id` field on every cell**
- ✅ Clean, minimal metadata — **no leftover widget state**, no Colab-specific metadata, no corrupted execution counts
- ✅ Executed top-to-bottom with a real kernel — **zero error outputs** in any of the 63 cells
- ✅ Verified with a raw `json.load()` round-trip, not just `nbformat`, to catch any low-level JSON corruption

---

## 📦 Deliverables

| File | Description |
|---|---|
| `Student_Performance_Intelligence_Platform_ADVANCED.ipynb` | The full, executed, 16-phase notebook (images embedded inline) |
| `Student_Performance_Intelligence_Platform_Artifacts.zip` | All 85 PNGs + 39 CSVs, organized by phase folder — ready to upload for document generation |
| `README.md` | This file |

---

<p align="center"><i>Built with rigor: every phase checked, every claim validated, every model explained.</i></p>
