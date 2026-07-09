<div align="center">

# ❤️‍🩹 Heart Attack Risk Prediction
### An End-to-End, Industry-Grade Machine Learning Methodology

**Statistical Rigor · Leak-Free Engineering · Multi-Tier Modeling · Explainable AI**

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![scikit--learn](https://img.shields.io/badge/scikit--learn-1.8-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-3.x-016A70?style=flat-square)
![LightGBM](https://img.shields.io/badge/LightGBM-4.x-9ACD32?style=flat-square)
![SHAP](https://img.shields.io/badge/SHAP-explainability-8A2BE2?style=flat-square)
![LIME](https://img.shields.io/badge/LIME-explainability-FF6F61?style=flat-square)
![Colab](https://img.shields.io/badge/Run%20on-Google%20Colab-F9AB00?style=flat-square&logo=googlecolab&logoColor=white)
![License](https://img.shields.io/badge/Data-Synthetic%20%2F%20Educational-lightgrey?style=flat-square)

</div>

---

## ⚠️ Disclaimer

> This project uses a **synthetically generated** dataset built to *mimic* real-world cardiac
> health patterns. It does **not** represent real patients and must **never** be used for actual
> medical diagnosis, triage, or clinical decision-making. Its purpose is strictly educational —
> a rigorous sandbox for practicing production-grade ML methodology.

---

## 📌 Project Snapshot

| | |
|---|---|
| **Objective** | Binary classification — predict `heart_attack_risk` (0 = Low, 1 = High) |
| **Dataset** | 7,000 patient records · 21 features · 11 columns with realistic missingness |
| **Notebook** | `Heart_Attack_Risk_Prediction_Final.ipynb` — single, self-contained, Colab-ready |
| **Methodology** | 16 sequential phases, EDA → Statistics → Feature Engineering → Modeling → Tuning → Explainability → Diagnostics |
| **Visual Artifacts** | 70 inline, report-ready figures (auto-numbered & saved as PNG) |
| **Tabular Artifacts** | 13 CSV exports covering audits, tests, importances, predictions |
| **Champion Model** | Tuned Linear SVM — **ROC-AUC 0.871** · Accuracy 78.9% · F1 0.733 |
| **Explainability** | SHAP (global + local) and LIME (per-instance) on the champion model |
| **Compute** | Auto GPU-detection for Colab; XGBoost switches to `device='cuda'` when available |

---

## 🗺️ Methodology at a Glance

```
Raw Data ──▶ Audit ──▶ Clean ──▶ EDA ──▶ Statistics ──▶ Feature Engineering
                                                              │
                                                              ▼
        Explainability ◀── Tuning ◀── Advanced Models ◀── Baseline Models ◀── Split + Pipeline
              │
              ▼
   Bias-Variance & Overfitting Diagnostics ──▶ Final Model ──▶ Artifact Export & ZIP
```

Every phase below maps 1:1 to a section in the notebook, so this README doubles as a
**table of contents** and a **methodology report**.

---

## 🧭 Phase-by-Phase Methodology

### Phase 1 — Environment Setup & GPU Activation 🖥️
Establishes a fully reproducible foundation before any analysis begins.
- Installs/imports the complete stack (`scikit-learn`, `xgboost`, `lightgbm`, `shap`, `lime`, `statsmodels`) with safe no-op checks.
- Fixes `RANDOM_STATE = 42` across NumPy, splits, and every model — identical results on every re-run.
- **GPU-aware by design:** probes `nvidia-smi`; if a Colab GPU runtime is active, `XGBoost` automatically switches to `device='cuda'`, otherwise it falls back to CPU with zero code changes required.
- Creates the artifact scaffolding (`artifacts/figures`, `artifacts/csv`, `artifacts/models`) and a numbered `save_and_show()` utility so every figure is simultaneously displayed inline *and* persisted for the final report.

### Phase 2 — Data Loading & Initial Audit 🔍
- Colab-aware loader: auto-detects the CSV locally or prompts an upload widget.
- Produces a structural audit (dtypes, missing %, cardinality) exported as CSV.
- Flags duplicate rows and profiles the target class balance.
- **Visuals:** missing-value bar chart, full-dataset missingness heatmap.

### Phase 3 — Data Cleaning & Preprocessing 🧹
- Deliberately maintains **two parallel data tracks**:
  - `df_eda` — globally imputed (median/mode) copy used *only* for visualization/statistics, where leakage isn't a modeling concern.
  - The **modeling data** — left untouched here; all imputation is deferred to a pipeline fit strictly on the training fold (Phase 7).
- Drops the non-predictive `patient_id` identifier.
- **Visual:** grid of outlier boxplots across all numeric features.

### Phase 4 — Exploratory Data Analysis (EDA) 📊 · *38 visuals*
The largest phase, built entirely from **reusable plotting functions** looped over feature lists — no copy-pasted plotting code.

| Sub-section | What it reveals |
|---|---|
| Univariate — numeric | Distribution shape, mean/median markers |
| Univariate — categorical | Class frequencies across 8 categorical fields |
| Bivariate — numeric vs. target | Boxplots showing risk-group separation |
| Bivariate — categorical vs. target | Stacked risk-rate bars per category |
| Correlation & multivariate | Heatmap, ranked target-correlation bars, pairplot, violin & scatter grids |

### Phase 5 — Advanced & Granular Statistical Analysis 🧪
Moves from visual impressions to formal, defensible evidence:
- **Chi-square test of independence** — every categorical feature vs. target.
  → `smoking_status`, `thalassemia`, `physical_activity`, `chest_pain_type`, `st_slope`, `alcohol_consumption` are statistically significant (p < 0.05); `gender` and `resting_ecg` are not.
- **Welch's t-test + Cohen's d** — effect size for every numeric feature.
- **Variance Inflation Factor (VIF)** — multicollinearity screen (all numeric features scored **well below 5**, confirming no problematic collinearity).
- **Skewness & kurtosis** — quantifies distribution shape ahead of modeling.

### Phase 6 — Feature Engineering 🛠️
A custom `ClinicalFeatureBuilder` (a proper `scikit-learn` `TransformerMixin`) derives clinically-motivated features using **fixed, literature-based thresholds** — not statistics learned from data — so it can safely sit *before* the train/test split with zero leakage risk.

| New Feature | Basis |
|---|---|
| `age_group` | Young adult / middle-aged / senior / elderly banding |
| `bp_category` | Standard hypertension staging |
| `cholesterol_category` | Desirable / borderline / high |
| `bmi_category` | WHO BMI classification |
| `heart_rate_reserve` | (220 − age) − observed max heart rate |
| `composite_risk_score` | Weighted blend of standardized clinical risk indicators |
| `comorbidity_count` | Family history + diabetes + fasting blood sugar + current smoking |

### Phase 7 — Train/Test Split & Leak-Free Pipeline 🔒
- Single **stratified 80/20 split**, reused identically by every model in the notebook.
- A `ColumnTransformer` fits `SimpleImputer` + `StandardScaler` (numeric) and `SimpleImputer` + `OneHotEncoder` (categorical) **exclusively on the training fold**, then transforms the test fold — the textbook leak-free pattern.

### Phase 8 — Baseline Models 📈
Five interpretable classifiers establish the performance floor: **Logistic Regression, K-Nearest Neighbors, Decision Tree, Gaussian Naive Bayes, Linear SVM.** A shared `evaluate_model()` utility logs Accuracy / Precision / Recall / F1 / ROC-AUC / Train-Accuracy consistently for every model in every tier.

### Phase 9 — Advanced Models 🌲
Ensemble & boosting methods: **Random Forest, Extra Trees, AdaBoost, Gradient Boosting, XGBoost (GPU-aware), LightGBM.**

### Phase 10 — Hyperparameter Tuning: Top 3 Models 🎯
- The three models with the highest test ROC-AUC are **auto-selected** — no hardcoding.
- Each is tuned via `RandomizedSearchCV` (stratified CV, ROC-AUC scoring) over a model-specific search space.
- Before/after comparison and 3-fold CV score-stability boxplots quantify the tuning gain.

### Phase 11 — Model Comparison: All Levels 🏆
A consolidated **leaderboard** spanning baseline → advanced → tuned tiers:

| Rank | Model | Tier | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|---|---|
| 🥇 | Linear SVM (Tuned) | tuned | 0.789 | 0.781 | 0.690 | 0.733 | **0.871** |
| 🥈 | Logistic Regression (Tuned) | tuned | 0.786 | 0.771 | 0.697 | 0.732 | 0.870 |
| 🥉 | Linear SVM | baseline | 0.788 | 0.775 | 0.697 | 0.734 | 0.870 |
| 4 | Logistic Regression | baseline | 0.786 | 0.772 | 0.696 | 0.732 | 0.869 |
| 5 | AdaBoost | advanced | 0.784 | 0.768 | 0.697 | 0.731 | 0.864 |
| … | *(9 more models — full detail in* `09_full_model_leaderboard.csv` *)* | | | | | | |

> The near-linear separability of this dataset means simple linear models edge out complex
> ensembles — a genuinely useful, evidence-based finding rather than an assumption.

### Phase 12 — Granular Feature Importance 🔬
- Native impurity/gain importances for tree-based members of the tuned top-3.
- **Permutation importance** (ROC-AUC drop, 10 repeats) on the champion model — the model-agnostic cross-check against tree-importance bias.

### Phase 13 — Explainability: SHAP 🧩
- Auto-selects the fastest valid explainer: `TreeExplainer` → `LinearExplainer` (for linear-coefficient models like the champion SVM) → bounded model-agnostic `Explainer` as a last resort.
- **Deliverables:** beeswarm summary, mean-|impact| bar chart, dependence plots for the top-2 drivers, and a single-prediction waterfall.

### Phase 14 — Explainability: LIME 🍋
- Local surrogate explanations for one correctly-classified **high-risk** case and one correctly-classified **low-risk** case — a complementary, instance-level lens next to SHAP's global consistency.

### Phase 15 — Bias-Variance Trade-off & Overfitting Diagnostics ⚖️
- **Learning curves** for the tuned top-3 — train vs. cross-validated score convergence.
- **Train-vs-test accuracy gap** scanned across *every* model in the leaderboard (tree ensembles show the largest gap — a textbook variance signature; linear models show the smallest).
- **Validation curve** on the champion model's core complexity hyperparameter, tracing the classic bias-variance U-shape.

### Phase 16 — Final Model Selection, Export & Download 📦
- Confusion matrix + classification-report heatmap for the champion model.
- Full test-set predictions exported to CSV.
- The **entire inference pipeline** (feature engineering → preprocessing → model) serialized with `joblib`.
- Every figure, CSV, and model artifact zipped into a single archive and auto-downloaded in Colab via `files.download()`.

---

## 📂 Repository / Artifact Structure

```
heart_attack_risk_project_artifacts.zip
├── figures/     → 70 numbered PNGs, one per phase-4-through-16 visual
├── csv/         → 13 CSVs: audits, statistical tests, importances, predictions
└── models/      → final_pipeline.joblib (feature engineering + preprocessing + model)
```

---

## ▶️ How to Run

1. Open `Heart_Attack_Risk_Prediction_Final.ipynb` in **Google Colab**.
2. *(Optional but recommended)* `Runtime → Change runtime type → T4 GPU`.
3. `Runtime → Run all`. Upload `heart_attack_dataset.csv` if prompted.
4. At completion, the artifact ZIP downloads automatically — ready to embed in a report or portfolio.

---

## 🧱 Engineering Principles Followed

- **Modularity** — reusable functions (`plot_numeric_distribution`, `evaluate_model`, `plot_learning_curve`, …) instead of repeated inline code.
- **Leak-free design** — every learned statistic (imputers, scaler, encoder, tuned hyperparameters) is fit on the training fold only; engineered features use fixed clinical thresholds, not data-derived ones.
- **Reproducibility** — a single global random seed; deterministic stratified splits and CV folds.
- **Traceability** — every figure and table is numbered and saved, mirroring the notebook's narrative order.
- **Portability** — GPU-aware but never GPU-*dependent*; runs identically on CPU-only environments.

---

<div align="center">

*Built for rigorous ML practice on synthetic clinical-style data — not for real-world diagnosis.*

</div>
