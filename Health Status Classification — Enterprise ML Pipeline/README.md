# 🩺 Health Status Classification — Enterprise ML Pipeline

> **AI-Powered Health & Lifestyle Analytics Framework**  
> Production-grade · Research-grade · Portfolio-ready · GitHub showcase compatible

---

## Project Overview

This project delivers an end-to-end machine learning pipeline for classifying individual health status (`Underweight`, `Healthy`, `Overweight`, `Obese`) from biometric and lifestyle features. The pipeline goes well beyond baseline classification — it integrates advanced feature engineering, Bayesian hyperparameter optimization, ensemble methods, and full explainability via SHAP.

**Use cases:** Preventive health analytics, personalised nutrition systems, population health monitoring, clinical decision support.

---

## Dataset

| Property | Value |
|----------|-------|
| Records | 6,000 individuals |
| Features | 15 (biometric + dietary + lifestyle) |
| Target | `Health_Status` (4 classes) |
| Missing values | None |
| Class balance | Overweight 41.7% · Healthy 37.5% · Obese 17.9% · Underweight 2.9% |

### Feature Reference

| Feature | Type | Description |
|---------|------|-------------|
| Age | Numeric | Individual age |
| Gender | Categorical | Male / Female / Other |
| Height_cm | Numeric | Height in centimetres |
| Weight_kg | Numeric | Body weight in kg |
| BMI | Numeric | Body Mass Index |
| Activity_Level | Categorical | Sedentary → Very Active |
| Daily_Calorie_Requirement | Numeric | Estimated TDEE |
| Daily_Calorie_Consumed | Numeric | Actual daily intake |
| Protein_Intake_g | Numeric | Daily protein |
| Carbohydrate_Intake_g | Numeric | Daily carbohydrates |
| Fat_Intake_g | Numeric | Daily fat |
| Water_Intake_Liters | Numeric | Daily hydration |
| Diet_Type | Categorical | Keto / Vegan / Balanced / High Protein / … |
| Health_Status | Target | Classification label |

---

## Pipeline Architecture

```
Raw Data (CSV)
    │
    ├── Phase 0  — Environment Init, Seed Management, Config, Logging
    ├── Phase 1  — Problem Understanding & Healthcare Context
    ├── Phase 2  — Data Quality Audit (profiling, missingness, duplicates)
    ├── Phase 3  — Advanced EDA (univariate, bivariate, interactive Plotly)
    ├── Phase 4  — Feature Engineering (20+ derived features)
    ├── Phase 5  — Feature Selection (MI + RF importance, top-K ranking)
    ├── Phase 6  — Preprocessing Pipeline (RobustScaler, ColumnTransformer)
    ├── Phase 7  — Baseline ML Framework (12 classifiers)
    ├── Phase 8  — Hyperparameter Optimization (Optuna + RandomizedSearchCV)
    ├── Phase 9  — Ensemble Learning (Soft Voting + Stacking)
    ├── Phase 10 — Comprehensive Evaluation (9 metrics, ROC curves)
    ├── Phase 11 — Explainable AI (SHAP global + local, permutation importance)
    └── Phase 12 — Artifact Export (models, reports, ZIP)
```

---

## Feature Engineering

20+ features engineered across 6 categories:

| Category | Features Created |
|----------|-----------------|
| BMI Derived | `BMI_Category_Eng`, `BMI_Risk_Score`, `BMI_Dev_Healthy` |
| Energy Balance | `Calorie_Surplus`, `Calorie_Surplus_Abs`, `Energy_Balance_Cat` |
| Macronutrient Ratios | `Protein_Pct`, `Carb_Pct`, `Fat_Pct`, `Macro_Balance_Score` |
| Lifestyle Encoding | `Activity_Intensity`, `Hydration_Adequate`, `Calorie_Density` |
| Statistical | Z-scores and percentile ranks for BMI, Calories, Protein |
| Interactions | `BMI_x_Activity`, `Age_BMI_Ratio` |

---

## ML Models

### Baseline (12 classifiers)
Linear: Logistic Regression, Ridge Classifier  
Distance: KNN  
Trees: Decision Tree, Random Forest, Extra Trees  
Boosting: AdaBoost, Gradient Boosting, HistGradientBoosting  
Advanced: XGBoost, LightGBM, CatBoost

### Optimized & Ensemble
- **XGBoost Tuned** — Optuna Bayesian search (30 trials, 9 hyperparameters)
- **LightGBM Tuned** — Optuna Bayesian search (30 trials)
- **CatBoost Tuned** — RandomizedSearchCV
- **Soft Voting Ensemble** — XGB + LGB + CatBoost
- **Stacking Ensemble** — RF + XGB + LGB → Logistic Regression meta-learner

---

## Hyperparameter Optimization

| Method | Models | Search Space |
|--------|--------|-------------|
| Optuna TPE (30 trials) | XGBoost, LightGBM | n_estimators, max_depth, learning_rate, subsample, colsample_bytree, regularization (L1/L2) |
| RandomizedSearchCV (15 iter) | CatBoost | depth, learning_rate, l2_leaf_reg |
| 3-fold CV | All | F1-Macro objective |

---

## Evaluation Metrics

| Metric | Description |
|--------|-------------|
| Accuracy | Overall correct predictions |
| F1-Macro | Unweighted mean per class (handles imbalance) |
| F1-Weighted | Weighted mean per class |
| Balanced Accuracy | Mean recall across classes |
| ROC-AUC (OvR) | Multi-class discrimination |
| Matthews Correlation Coefficient | Best single metric for imbalanced multi-class |
| Cohen's Kappa | Agreement beyond chance |

---

## Explainable AI (SHAP)

- **Global summary plot** — feature impact distribution across all test samples
- **Bar importance** — mean |SHAP| per feature ranked across all classes
- **Local waterfall plots** — per-sample explanations for 3 representative cases
- **Permutation importance** — model-agnostic validation of feature stability

Key finding: BMI, Calorie Surplus, BMI_Dev_Healthy, and Activity_Intensity are consistently the top 4 drivers across all explanation methods.

---

## Output Folder Structure

```
Health_Status_AI_Project/outputs/
├── eda/
│   ├── univariate_distributions.png
│   ├── boxplots_outliers.png
│   ├── categorical_composition.png
│   ├── target_distribution.png
│   ├── nutrition_by_health.png
│   ├── correlation_heatmap.png
│   └── parallel_coordinates.html
├── features/
│   └── feature_importance.png
├── preprocessing/
│   └── scaler.pkl
├── models/
│   ├── xgb_tuned.pkl
│   ├── lgb_tuned.pkl
│   ├── cb_tuned.pkl
│   ├── stacking_ensemble.pkl
│   └── label_encoder.pkl
├── evaluation/
│   ├── baseline_leaderboard.png
│   ├── baseline_results.csv
│   ├── confusion_matrix.png
│   ├── roc_curves.png
│   ├── full_leaderboard.png
│   ├── all_models_comparison.csv
│   └── final_metrics.csv
├── xai/
│   ├── shap_summary.png
│   ├── shap_bar_importance.png
│   ├── shap_waterfall_case1.png
│   ├── shap_waterfall_case2.png
│   ├── shap_waterfall_case3.png
│   └── permutation_importance.csv
└── reports/
    └── executive_report.txt
```

---

## Usage Instructions

### Requirements

```
pandas numpy matplotlib seaborn plotly scikit-learn
xgboost lightgbm catboost optuna shap joblib
```

Install:
```bash
pip install xgboost lightgbm catboost optuna shap plotly
```

### Running the Notebook

1. Upload `healthy_diet_calorie_intake.csv` to your runtime
2. Set `DATA_PATH` in Phase 0 config:
   ```python
   CONFIG["data_path"] = "/content/Health_Status_Dataset.csv"
   ```
3. Run all cells (`Runtime → Run all` in Colab, or `Kernel → Restart & Run All` in Jupyter)
4. Download `Health_Status_Classification_Project.zip` for all outputs

### Loading a Saved Model

```python
import joblib, numpy as np

model   = joblib.load("outputs/models/xgb_tuned.pkl")
scaler  = joblib.load("outputs/preprocessing/scaler.pkl")
le      = joblib.load("outputs/models/label_encoder.pkl")

# New individual: [Age, BMI, CalorieConsumed, ...]
X_new   = np.array([[32, 27.5, 2200, 80, 200, 70, 2.5, 3]])
X_scaled = scaler.transform(X_new)
pred     = le.inverse_transform(model.predict(X_scaled))
print(f"Predicted health status: {pred[0]}")
```

---

## Key Results

| Model | Accuracy | F1-Macro | MCC |
|-------|----------|----------|-----|
| Logistic Regression (baseline) | ~0.85 | ~0.72 | ~0.70 |
| Random Forest (baseline) | ~0.97 | ~0.94 | ~0.93 |
| XGBoost Tuned | ~0.98 | ~0.96 | ~0.95 |
| LightGBM Tuned | ~0.98 | ~0.96 | ~0.95 |
| Stacking Ensemble | **~0.99** | **~0.97** | **~0.96** |

*Results may vary slightly by run due to stochastic elements; SEED=42 for reproducibility.*

---

## Future Improvements

- **SMOTE / class weighting** for the minority Underweight class (2.9%)
- **Neural network baseline** (PyTorch MLP) for comparison
- **AutoML integration** (AutoGluon or FLAML) for further search
- **Federated learning** approach for privacy-preserving health analytics
- **Real-time API** wrapper (FastAPI) with SHAP explanation endpoint
- **Longitudinal tracking** with time-series features if repeat measurements available
- **Clinical validation** against ICD-10 coded patient records

---

## Tech Stack

| Tool | Version | Purpose |
|------|---------|---------|
| Python | 3.10+ | Core runtime |
| scikit-learn | 1.4+ | Baseline models, pipelines, metrics |
| XGBoost | 2.0+ | Gradient boosting (GPU-ready) |
| LightGBM | 4.0+ | Fast gradient boosting |
| CatBoost | 1.2+ | Categorical-native boosting |
| Optuna | 3.0+ | Bayesian hyperparameter optimization |
| SHAP | 0.44+ | Model explainability |
| Plotly | 5.0+ | Interactive visualizations |

---

*This pipeline represents a complete AI-driven health analytics platform — not a simple classification exercise.*
