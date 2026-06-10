<div align="center">

<br/>

```
███████╗████████╗██╗   ██╗██████╗ ██╗   ██╗    ██╗      █████╗ ██████╗ ███████╗
██╔════╝╚══██╔══╝██║   ██║██╔══██╗╚██╗ ██╔╝    ██║     ██╔══██╗██╔══██╗██╔════╝
███████╗   ██║   ██║   ██║██║  ██║ ╚████╔╝     ██║     ███████║██████╔╝███████╗
╚════██║   ██║   ██║   ██║██║  ██║  ╚██╔╝      ██║     ██╔══██║██╔══██╗╚════██║
███████║   ██║   ╚██████╔╝██████╔╝   ██║       ███████╗██║  ██║██████╔╝███████║
╚══════╝   ╚═╝    ╚═════╝ ╚═════╝    ╚═╝       ╚══════╝╚═╝  ╚═╝╚═════╝ ╚══════╝
```

# 🎓 Student Habits vs Academic Performance
### End-to-End Machine Learning Pipeline · Advanced EDA · XAI / SHAP · Ensemble Orchestration

<br/>

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.4%2B-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.0%2B-FF6600?style=for-the-badge)](https://xgboost.readthedocs.io/)
[![LightGBM](https://img.shields.io/badge/LightGBM-4.3%2B-02569B?style=for-the-badge)](https://lightgbm.readthedocs.io/)
[![SHAP](https://img.shields.io/badge/SHAP-0.45%2B-FF4081?style=for-the-badge)](https://shap.readthedocs.io/)
[![Optuna](https://img.shields.io/badge/Optuna-3.6%2B-5C4EE5?style=for-the-badge)](https://optuna.org/)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

<br/>

> *"The goal of ML is not just to predict — but to understand, explain, and improve."*

<br/>

---

</div>

## 📋 Table of Contents

- [Overview](#-overview)
- [Dataset](#-dataset)
- [Project Architecture](#-project-architecture)
- [Pipeline Phases](#-pipeline-phases)
- [Models Implemented](#-models-implemented)
- [Explainable AI](#-explainable-ai--xai)
- [Hyperparameter Tuning](#-hyperparameter-tuning)
- [Outputs & Artefacts](#-outputs--artefacts)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)
- [Key Results](#-key-results)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🔭 Overview

This project presents a **production-grade, end-to-end machine learning pipeline** that investigates how student lifestyle habits — study hours, sleep, screen time, diet quality, mental health, and more — influence academic performance (final exam scores).

The pipeline spans the full ML lifecycle: rigorous exploratory analysis → advanced feature engineering → a multi-framework model comparison (16 regression + 18 classification models) → Optuna-powered hyperparameter optimisation → stacking/voting ensemble orchestration → and finally, a rich Explainable AI layer using SHAP, permutation importance, and native tree-based interpretability.

All 28+ visualisations, model metrics, SHAP values, and tuning artefacts are automatically exported, zipped, and made available for download at the end of the notebook run.

<div align="center">

```
Raw CSV  ──▶  Advanced EDA  ──▶  Feature Engineering  ──▶  Preprocessing
                                                                  │
      XAI / SHAP  ◀──  Ensemble Orchestration  ◀──  Tuning  ◀──  Models
                                                                  │
                          Output Archive (ZIP)  ◀────────────────┘
```

</div>

---

## 📊 Dataset

| Property | Value |
|---|---|
| **Name** | Student Habits vs Academic Performance |
| **Records** | 1 000 synthetic student entries |
| **Features** | 15+ lifestyle, demographic & academic variables |
| **Regression Target** | `exam_score` (continuous, 18.4 – 100.0) |
| **Classification Target** | `grade_band` (5-class: F / D / C / B / A) |
| **Missing Data** | `parental_education_level` (~9.1%) — imputed via mode |
| **Source** | Synthetic, designed for educational ML practice |

### Feature Inventory

```
Numeric                          Categorical
─────────────────────────────    ──────────────────────────────────
age                              gender               [Male / Female / Other]
study_hours_per_day              part_time_job        [Yes / No]
social_media_hours               diet_quality         [Poor / Fair / Good]
netflix_hours                    parental_education   [High School / Bachelor / Master]
attendance_percentage            internet_quality     [Poor / Average / Good]
sleep_hours                      extracurricular      [Yes / No]
exercise_frequency
mental_health_rating  (1–10)
exam_score            ← TARGET
```

---

## 🏗 Project Architecture

```
student_habits_ML_advanced.ipynb
│
├── Phase 00 ── Environment Setup & Dependency Installation
├── Phase 01 ── Data Loading & Initial Inspection
├── Phase 02 ── Advanced Exploratory Data Analysis
│              ├── Univariate distributions (numeric + categorical)
│              ├── Correlation heatmap
│              ├── Target variable deep-dive
│              ├── Interactive Plotly scatter matrix
│              ├── Bivariate scatter (grouped by lifestyle categories)
│              ├── Violin plots (mental health × exam score)
│              └── Pivot heatmap (diet × internet quality)
│
├── Phase 03 ── Advanced Feature Engineering
│              ├── Ordinal & binary encodings
│              ├── 12 derived interaction features
│              ├── Power-transformed skewed variables
│              ├── Polynomial terms
│              └── Grade-band classification target
│
├── Phase 04 ── Preprocessing Pipeline (StandardScaler · Imputation · Split)
│
├── Phase 05 ── Regression Framework   [16 models]
├── Phase 06 ── Classification Framework [18 models]
│
├── Phase 07 ── Ultra-Advanced Tuning
│              ├── Optuna TPE (60 trials) — XGBoost Regressor
│              ├── Optuna TPE (60 trials) — LightGBM Classifier
│              └── RandomizedSearchCV (30 iter) — CatBoost Classifier
│
├── Phase 08 ── Ensemble Orchestration
│              ├── Stacking Regressor  (4 base learners + Ridge meta)
│              ├── Stacking Classifier (4 base learners + LR meta)
│              └── Soft Voting Classifier
│
├── Phase 09 ── Explainable AI (SHAP · Permutation · Native Importance)
│
├── Phase 10 ── Cross-Validation & Final Report
│
└── Phase 11 ── Output Export & ZIP Archive Download
```

---

## 🔄 Pipeline Phases

<details>
<summary><strong>Phase 0 — Environment Setup</strong></summary>

Programmatically installs and upgrades all required dependencies at runtime. No manual `pip install` steps are needed before running the notebook. Packages are pinned to minimum versions to ensure reproducibility.

</details>

<details>
<summary><strong>Phase 1 — Data Loading & Inspection</strong></summary>

Loads the CSV from the session local storage. Performs dtype profiling, shape inspection, and a structured missing-value audit with percentage breakdown per column.

</details>

<details>
<summary><strong>Phase 2 — Advanced EDA</strong></summary>

Eight publication-quality visualisations covering:

- **Histogram grids** with mean annotations for all numeric features
- **Bar chart grid** for all categorical columns with count labels
- **Triangular correlation heatmap** (diverging colour scale, annotated)
- **Target distribution** — KDE overlay, gender boxplot, diet quality boxplot
- **Interactive Plotly scatter matrix** (exported to HTML for full interactivity)
- **Study hours vs exam score** grouped scatter by diet and internet quality
- **Violin plots** by mental health rating (1–10)
- **Pivot heatmap** — mean exam score by diet × internet quality

Every plot includes an interpretation comment block explaining key findings.

</details>

<details>
<summary><strong>Phase 3 — Advanced Feature Engineering</strong></summary>

| Feature | Formula / Logic |
|---|---|
| `screen_time` | `social_media_hours + netflix_hours` |
| `study_efficiency` | `study_hours × attendance / 100` |
| `lifestyle_score` | Weighted composite of sleep, exercise, diet, mental health |
| `pressure_index` | `study_hours / (sleep_hours + ε)` |
| `study_screen_ratio` | `study_hours / (screen_time + ε)` |
| `wellbeing` | Weighted composite of mental health, sleep, exercise |
| `study_x_mh` | Interaction: `study_hours × mental_health_rating` |
| `study_sq` | Polynomial: `study_hours²` |
| `sleep_sq` | Polynomial: `sleep_hours²` |
| `diet_enc` | Ordinal encoding: Poor=0, Fair=1, Good=2 |
| `edu_enc` | Ordinal encoding: High School=0, Bachelor=1, Master=2 |
| `inet_enc` | Ordinal encoding: Poor=0, Average=1, Good=2 |

Power transformation (Yeo-Johnson) applied to skewed derived features.

</details>

<details>
<summary><strong>Phase 4 — Preprocessing</strong></summary>

- Mode imputation for `parental_education_level`
- `StandardScaler` fit on training set only (no data leakage)
- 80/20 train/test split with `random_state=42`
- Stratified split for classification target

</details>

<details>
<summary><strong>Phase 5 & 6 — Model Frameworks</strong></summary>

See [Models Implemented](#-models-implemented) section below.

</details>

<details>
<summary><strong>Phase 7 — Hyperparameter Tuning</strong></summary>

See [Hyperparameter Tuning](#-hyperparameter-tuning) section below.

</details>

<details>
<summary><strong>Phase 8 — Ensemble Orchestration</strong></summary>

Three ensemble strategies are constructed and evaluated:

- **Stacking Regressor** — Ridge + Random Forest + XGBoost + LightGBM base learners, with Ridge as the meta-learner (5-fold cross-stacking to prevent leakage)
- **Stacking Classifier** — same base architecture with Logistic Regression as meta-learner, using `predict_proba` stacking
- **Soft Voting Classifier** — probability-averaged ensemble of Random Forest, XGBoost, and LightGBM

</details>

<details>
<summary><strong>Phase 9 — Explainable AI</strong></summary>

See [Explainable AI](#-explainable-ai--xai) section below.

</details>

<details>
<summary><strong>Phase 10 — Cross-Validation & Report</strong></summary>

5-fold cross-validation on top-4 regressors with boxplot visualisation. Full summary table (all models, both tasks) exported to CSV. Radar chart comparing top regression models across normalised R² and RMSE metrics.

</details>

<details>
<summary><strong>Phase 11 — Export & Download</strong></summary>

All output files (28+ plots, CSVs, JSON param files) are zipped into `student_habits_ml_outputs.zip` and auto-downloaded. No manual file collection required.

</details>

---

## 🤖 Models Implemented

### Regression (16 Models)

| # | Model | Regularisation |
|---|---|---|
| 1 | Linear Regression | — |
| 2 | Ridge Regression | L2 |
| 3 | Lasso Regression | L1 |
| 4 | ElasticNet | L1 + L2 |
| 5 | Huber Regressor | Robust / outlier-resistant |
| 6 | Bayesian Ridge | Bayesian prior |
| 7 | K-Nearest Neighbours | — |
| 8 | Decision Tree | `max_depth` |
| 9 | Random Forest | `max_depth`, `n_estimators` |
| 10 | Extra Trees | `max_depth`, `n_estimators` |
| 11 | Gradient Boosting (sklearn) | Shrinkage, `max_depth` |
| 12 | AdaBoost | — |
| 13 | Support Vector Regressor | C, ε |
| 14 | **XGBoost** | L1 (`reg_alpha`), L2 (`reg_lambda`) |
| 15 | **LightGBM** | L1, L2, `num_leaves` |
| 16 | **CatBoost** | L2 (`l2_leaf_reg`), depth |
| ✦ | **Stacking Ensemble** | Meta-learner: Ridge |
| ✦ | **XGBoost (Optuna-Tuned)** | Full search over 9 hyperparameters |

### Classification (18 Models)

| # | Model | Variant |
|---|---|---|
| 1 | Logistic Regression | L2 (default) |
| 2 | Logistic Regression | L1 (`saga` solver) |
| 3 | Logistic Regression | ElasticNet |
| 4 | Linear Discriminant Analysis | — |
| 5 | Quadratic Discriminant Analysis | — |
| 6 | Gaussian Naive Bayes | — |
| 7 | K-Nearest Neighbours | — |
| 8 | Decision Tree | `max_depth` |
| 9 | Random Forest | `max_depth`, `n_estimators` |
| 10 | Extra Trees | — |
| 11 | Gradient Boosting (sklearn) | Shrinkage |
| 12 | AdaBoost | — |
| 13 | SVC (RBF kernel) | C, γ |
| 14 | SVC (Linear kernel) | C |
| 15 | SGD Classifier | Modified Huber loss |
| 16 | **XGBoost** | L1, L2 |
| 17 | **LightGBM** | L1, L2 |
| 18 | **CatBoost** | L2 leaf |
| ✦ | **Stacking Classifier** | Meta-learner: Logistic Regression |
| ✦ | **Voting Classifier** | Soft vote (RF + XGB + LGB) |
| ✦ | **LightGBM (Optuna-Tuned)** | Full search over 9 hyperparameters |
| ✦ | **CatBoost (RandomSearch-Tuned)** | 30-iteration search |

---

## 🔍 Explainable AI / XAI

The interpretability layer is built on four complementary methods:

```
┌─────────────────────────────────────────────────────────────┐
│                  EXPLAINABILITY STACK                       │
├─────────────────┬───────────────────────────────────────────┤
│  SHAP           │  TreeExplainer on XGBoost (regression)    │
│  (Global)       │  Beeswarm summary — all features          │
│                 │  Bar chart — mean |SHAP| ranking          │
├─────────────────┼───────────────────────────────────────────┤
│  SHAP           │  Dependence plots — top-3 features        │
│  (Conditional)  │  Interaction effects with colour coding   │
├─────────────────┼───────────────────────────────────────────┤
│  SHAP           │  Waterfall plot — single sample explained │
│  (Local)        │  Feature contributions per student record │
├─────────────────┼───────────────────────────────────────────┤
│  SHAP           │  TreeExplainer on LightGBM (classifier)   │
│  (Multi-class)  │  Per-class SHAP decomposition             │
├─────────────────┼───────────────────────────────────────────┤
│  Permutation    │  15-repeat permutation importance         │
│  Importance     │  Mean decrease in R² ± std error bars     │
├─────────────────┼───────────────────────────────────────────┤
│  Native Tree    │  Gain-based importance (XGBoost & LGB)    │
│  Importance     │  Side-by-side bar chart comparison        │
└─────────────────┴───────────────────────────────────────────┘
```

Each SHAP plot includes an interpretation comment block directly in the notebook, narrating *what the output means* in the context of student performance.

---

## ⚙️ Hyperparameter Tuning

### Optuna (TPE Sampler) — XGBoost Regressor

| Parameter | Search Space |
|---|---|
| `n_estimators` | 100 – 600 (int) |
| `max_depth` | 3 – 10 (int) |
| `learning_rate` | 0.01 – 0.30 (log-uniform) |
| `subsample` | 0.5 – 1.0 (float) |
| `colsample_bytree` | 0.5 – 1.0 (float) |
| `reg_alpha` (L1) | 1e-4 – 10 (log-uniform) |
| `reg_lambda` (L2) | 1e-4 – 10 (log-uniform) |
| `min_child_weight` | 1 – 10 (int) |
| `gamma` | 0 – 5 (float) |
| **Trials** | **60** |
| **Objective** | 5-fold CV R² (maximise) |

### Optuna (TPE Sampler) — LightGBM Classifier

Same parameter set plus `num_leaves` (20–150) and `min_child_samples` (5–50).  
**60 trials**, objective: 5-fold stratified CV F1-weighted (maximise).

### RandomizedSearchCV — CatBoost Classifier

| Parameter | Distribution |
|---|---|
| `iterations` | Uniform int 100–400 |
| `learning_rate` | Uniform 0.01–0.30 |
| `depth` | Uniform int 3–10 |
| `l2_leaf_reg` | Uniform 1–10 |
| `border_count` | Uniform int 32–255 |
| **Iterations** | **30** · 5-fold stratified CV |

---

## 📦 Outputs & Artefacts

After a full pipeline run, the following files are available in the auto-generated `outputs/` directory and bundled into `student_habits_ml_outputs.zip`:

| # | File | Description |
|---|---|---|
| 01 | `01_univariate_numeric.png` | Histogram grid — all numeric features |
| 02 | `02_univariate_categorical.png` | Bar chart grid — all categorical features |
| 03 | `03_correlation_heatmap.png` | Annotated triangular correlation matrix |
| 04 | `04_target_deep_dive.png` | Exam score KDE, gender & diet boxplots |
| 05 | `05_pairplot_interactive.html` | Plotly scatter matrix (fully interactive) |
| 06 | `06_bivariate_study_vs_score.png` | Study hours vs score by lifestyle group |
| 07 | `07_violin_mental_health.png` | Violin plots by mental health rating |
| 08 | `08_heatmap_diet_internet.png` | Pivot heatmap — diet × internet quality |
| 09 | `09_grade_band_distribution.png` | Classification target class distribution |
| 10 | `10_feature_correlation_bar.png` | Correlation bar chart incl. engineered feats |
| 11 | `11_regression_model_comparison.png` | All 16 models — R², RMSE, MAE bars |
| 12 | `12_actual_vs_predicted_top3.png` | Scatter plots — top-3 regressors |
| 13 | `13_residual_analysis.png` | Residuals vs fitted, histogram, Q-Q |
| 14 | `14_classification_comparison.png` | All 18 models — Accuracy & F1 bars |
| 15 | `15_confusion_matrix.png` | Best classifier confusion matrix |
| 16 | `16_roc_curves.png` | One-vs-Rest ROC per grade band |
| 17 | `17_optuna_xgb_history.png` | Optuna optimisation history |
| 18 | `18_optuna_lgb_param_importance.png` | Optuna hyperparameter importances |
| 19 | `19_tuned_model_comparison.png` | Tuned vs baseline comparison chart |
| 20 | `20_shap_summary_beeswarm.png` | SHAP beeswarm — XGBoost regressor |
| 21 | `21_shap_bar.png` | SHAP mean absolute value bar chart |
| 22 | `22_shap_dependence.png` | SHAP dependence — top-3 features |
| 23 | `23_shap_waterfall.png` | SHAP waterfall — single student explained |
| 24 | `24_shap_classifier.png` | SHAP summary — LightGBM classifier |
| 25 | `25_permutation_importance.png` | Permutation importance with error bars |
| 26 | `26_native_feature_importance.png` | XGBoost & LightGBM gain importance |
| 27 | `27_cv_boxplot.png` | 5-fold CV R² distribution boxplot |
| 28 | `28_radar_top_models.png` | Radar chart — top regression models |
| — | `full_model_summary.csv` | All models, both tasks, all metrics |
| — | `regression_model_results.csv` | Regression results table |
| — | `classification_model_results.csv` | Classification results table |
| — | `shap_feature_importance.csv` | Mean absolute SHAP values per feature |
| — | `permutation_importance.csv` | Permutation importance mean ± std |
| — | `optuna_xgb_best_params.json` | Best XGBoost hyperparameters (JSON) |
| — | `optuna_lgb_best_params.json` | Best LightGBM hyperparameters (JSON) |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10 or higher
- Jupyter Notebook / JupyterLab environment  
  *(Google Colab, Kaggle Notebooks, or local Jupyter all supported)*

### Running the Notebook

**Step 1 — Upload the dataset**

Place `student_habits_performance.csv` in the same directory as the notebook, or upload it to the session storage via the file browser in your notebook environment.

**Step 2 — Open the notebook**

```bash
jupyter notebook student_habits_ML_advanced.ipynb
```

**Step 3 — Run all cells**

Select **Kernel → Restart & Run All** (or equivalent in your environment).

> All dependencies are installed automatically in Phase 0. No prior `pip install` is required.

**Step 4 — Download your outputs**

At the end of Phase 11, `student_habits_ml_outputs.zip` is created containing all 28+ visualisations, CSV exports, and JSON parameter files. In notebook environments with file download support, the download is triggered automatically.

---

## 📁 Project Structure

```
📦 student-habits-ml/
├── 📓 student_habits_ML_advanced.ipynb   ← Main pipeline notebook
├── 📄 student_habits_performance.csv      ← Dataset (upload before running)
├── 📘 README.md                           ← This file
├── 📜 LICENSE                             ← MIT License
└── 📂 outputs/                            ← Auto-generated on run
    ├── 🖼️  *.png                           ← 28 visualisation exports
    ├── 🌐 *.html                          ← Interactive Plotly charts
    ├── 📊 *.csv                           ← Model metrics & feature importance
    ├── 🔧 *.json                          ← Tuning artefacts (best params)
    └── 🗜️  student_habits_ml_outputs.zip  ← Complete archive
```

---

## 🛠 Tech Stack

<div align="center">

| Layer | Libraries |
|---|---|
| **Data** | `pandas` · `numpy` |
| **Visualisation** | `matplotlib` · `seaborn` · `plotly` |
| **Preprocessing** | `scikit-learn` (StandardScaler, PowerTransformer, LabelEncoder) |
| **Classical ML** | `scikit-learn` (Linear, Tree, Ensemble, SVM, Bayes) |
| **Gradient Boosting** | `xgboost` · `lightgbm` · `catboost` |
| **Ensemble** | `StackingRegressor` · `StackingClassifier` · `VotingClassifier` |
| **Hyperparameter Optimisation** | `optuna` (TPE) · `RandomizedSearchCV` |
| **Explainability** | `shap` (TreeExplainer, Beeswarm, Waterfall, Dependence) |
| **Validation** | `cross_val_score` · `StratifiedKFold` |
| **Export** | `zipfile` · `pathlib` |

</div>

---

## 📈 Key Results

> *Exact numbers vary by environment and random state. The values below reflect typical pipeline performance.*

### Regression (exam_score prediction)

| Model | R² | RMSE |
|---|---|---|
| XGBoost (Optuna-Tuned) | **~0.91** | **~4.9** |
| Stacking Regressor | ~0.90 | ~5.1 |
| LightGBM | ~0.89 | ~5.3 |
| Random Forest | ~0.87 | ~5.8 |
| Ridge Regression | ~0.63 | ~9.8 |

### Classification (grade_band prediction)

| Model | Accuracy | F1 (weighted) |
|---|---|---|
| LightGBM (Optuna-Tuned) | **~0.88** | **~0.88** |
| Stacking Classifier | ~0.87 | ~0.87 |
| XGBoost | ~0.86 | ~0.86 |
| Voting Classifier | ~0.86 | ~0.86 |
| Logistic Regression | ~0.71 | ~0.70 |

### Top SHAP Features (exam_score)

```
1. study_efficiency          ████████████████████  (strongest positive driver)
2. study_hours_per_day       ███████████████████
3. attendance_percentage     ██████████████████
4. screen_time               ██████████████       (negative impact)
5. social_media_hours        █████████████        (negative impact)
6. mental_health_rating      ████████████
7. wellbeing                 ██████████
8. sleep_hours               █████████
```

---

## 🤝 Contributing

Contributions are welcome. To contribute:

1. **Fork** the repository
2. Create a **feature branch** (`git checkout -b feature/your-feature-name`)
3. **Commit** your changes (`git commit -m 'feat: add new model / analysis'`)
4. **Push** to your branch (`git push origin feature/your-feature-name`)
5. Open a **Pull Request** with a clear description of the change

Please follow existing code style (PEP 8, modular functions, descriptive comments) and ensure new cells are phase-labelled consistently.

---

## 📄 License

This project is released under the **MIT License**.  
See the [LICENSE](LICENSE) file for full terms.

---

<div align="center">

<br/>

**Built with precision. Explained with clarity. Engineered for insight.**

<br/>

*If this project helped you — give it a ⭐ and share it with your network.*

<br/>

[![Made with Python](https://img.shields.io/badge/Made%20with-Python-3776AB?style=flat-square&logo=python)](https://python.org)
[![Powered by SHAP](https://img.shields.io/badge/Powered%20by-SHAP-FF4081?style=flat-square)](https://shap.readthedocs.io)
[![Tuned with Optuna](https://img.shields.io/badge/Tuned%20with-Optuna-5C4EE5?style=flat-square)](https://optuna.org)

</div>
