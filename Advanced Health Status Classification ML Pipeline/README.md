
<div align="center">

```
██████╗ ██╗███████╗████████╗     █████╗ ██╗
██╔══██╗██║██╔════╝╚══██╔══╝    ██╔══██╗██║
██║  ██║██║█████╗     ██║       ███████║██║
██║  ██║██║██╔══╝     ██║       ██╔══██║██║
██████╔╝██║███████╗   ██║       ██║  ██║██║
╚═════╝ ╚═╝╚══════╝   ╚═╝       ╚═╝  ╚═╝╚═╝
```

# 🥗 Healthy Diet Recipe Classification & Analytics Pipeline

**An enterprise-grade, research-quality machine learning framework for nutritional intelligence —
classifying diet types from macronutrient profiles with full explainability, automated tuning, and production-ready artifact management.**

<br/>

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.x-FF6600?style=for-the-badge&logo=xgboost&logoColor=white)](https://xgboost.readthedocs.io)
[![LightGBM](https://img.shields.io/badge/LightGBM-4.x-00B388?style=for-the-badge)](https://lightgbm.readthedocs.io)
[![CatBoost](https://img.shields.io/badge/CatBoost-1.x-FFCC00?style=for-the-badge&logoColor=black)](https://catboost.ai)
[![SHAP](https://img.shields.io/badge/SHAP-XAI-8B5CF6?style=for-the-badge)](https://shap.readthedocs.io)
[![Optuna](https://img.shields.io/badge/Optuna-Hyperopt-2196F3?style=for-the-badge)](https://optuna.org)
[![License](https://img.shields.io/badge/License-MIT-22C55E?style=for-the-badge)](LICENSE)

<br/>

> *"From three macros to production-grade intelligence — every recipe tells a nutritional story."*

</div>

---

## ◈ Table of Contents

- [Overview](#-overview)
- [Dataset](#-dataset)
- [Pipeline Architecture](#-pipeline-architecture)
- [Feature Engineering](#-feature-engineering)
- [Models & Performance](#-models--performance)
- [Hyperparameter Optimisation](#-hyperparameter-optimisation)
- [Explainable AI](#-explainable-ai)
- [Folder Structure](#-folder-structure)
- [Quick Start](#-quick-start)
- [Orchestration Layer](#-orchestration-layer)
- [Visualisations](#-visualisations)
- [Future Roadmap](#-future-roadmap)
- [Contributing](#-contributing)

---

## ◈ Overview

This project transforms raw macronutrient data from 7,806 diet recipes into a fully automated, explainable, and production-ready classification system. Five distinct dietary philosophies — **Mediterranean**, **DASH**, **Vegan**, **Ketogenic**, and **Paleo** — are learned from protein, carbohydrate, and fat profiles alone, enriched by 17 engineered nutritional features.

The pipeline spans the complete machine learning lifecycle: from data quality auditing and interactive visual analytics, through ensemble modelling and Bayesian hyperparameter search, to SHAP-based interpretability and artifact-packaged deployment.

**Built for:**
- 📊 Data scientists seeking a reference-quality multi-class classification framework  
- 🥦 Nutritional intelligence platforms requiring automated recipe categorisation  
- 🔬 Researchers studying macro-level dietary pattern discrimination  
- 🏗️ ML engineers demonstrating end-to-end pipeline architecture

---

## ◈ Dataset

### Source Files

| File | Recipes | Diet Category | Description |
|------|--------:|--------------|-------------|
| `All_Diets.csv` | **7,806** | Combined master | All five diets unified |
| `mediterranean.csv` | 1,753 | Mediterranean | Olive oil, legumes, fish, whole grains |
| `dash.csv` | 1,745 | DASH | Low-sodium, heart-healthy focus |
| `vegan.csv` | 1,522 | Vegan | Zero animal products |
| `keto.csv` | 1,512 | Ketogenic | High fat, very low carbohydrate |
| `paleo.csv` | 1,274 | Paleo | Ancestral whole-food patterns |

### Raw Features

| Column | Type | Description |
|--------|------|-------------|
| `Recipe_name` | string | Unique recipe identifier |
| `Cuisine_type` | categorical | 19 cuisine origins (American, Mediterranean, Italian…) |
| `Protein(g)` | float | Grams of protein per full recipe |
| `Carbs(g)` | float | Grams of carbohydrates per full recipe |
| `Fat(g)` | float | Grams of fat per full recipe |
| `Extraction_day` | date | Scrape date (excluded from model) |
| `Diet_type` | **target** | One of 5 diet labels |

### Class Distribution

```
mediterranean  ████████████████████  1,753  (22.5%)
dash           ███████████████████   1,745  (22.4%)
vegan          █████████████████     1,522  (19.5%)
keto           █████████████████     1,512  (19.4%)
paleo          ██████████████        1,274  (16.3%)

Imbalance ratio: 1.38  →  Near-balanced; class weights applied
```

---

## ◈ Pipeline Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                    17-PHASE PIPELINE OVERVIEW                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Phase 0   Environment Init · Seed Management · System Diagnostics  │
│     │                                                                │
│  Phase 1   Problem Framing · Dataset Inventory · ML Objectives      │
│     │                                                                │
│  Phase 2   Data Quality · Missing Analysis · Duplicate Detection    │
│     │                                                                │
│  Phase 3   EDA · Univariate · Bivariate · Radar · 3D Plotly        │
│     │                                                                │
│  Phase 4   Feature Engineering (20 nutritional features)            │
│     │                                                                │
│  Phase 5   Feature Selection · ANOVA · MI · RF · ET Consensus       │
│     │                                                                │
│  Phase 6   Preprocessing · ColumnTransformer · Leakage-Free Split   │
│     │                                                                │
│  Phase 7   Baseline Models (10 algorithms)                          │
│     │                                                                │
│  Phase 8   Advanced Boosting · XGBoost · LightGBM · CatBoost (GPU) │
│     │                                                                │
│  Phase 9   Hyperparameter Optimisation · Optuna · RandomizedSearch  │
│     │                                                                │
│  Phase 10  Regularisation Study · L1/L2/ElasticNet · Depth Sweep   │
│     │                                                                │
│  Phase 11  Ensemble · Soft/Hard Voting · RF+XGB+CB Stacking        │
│     │                                                                │
│  Phase 12  Evaluation · 9 Metrics · Confusion · ROC · PR Curves    │
│     │                                                                │
│  Phase 13  Explainability · SHAP Global · Waterfall · Permutation  │
│     │                                                                │
│  Phase 14  Orchestration Classes · DataManager · ModelTrainer      │
│     │                                                                │
│  Phase 15  Artifact Management · .joblib · CSV · HTML              │
│     │                                                                │
│  Phase 16  Executive + Technical Report Generation                  │
│     │                                                                │
│  Phase 17  ZIP Export · Complete Project Bundle                     │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## ◈ Feature Engineering

Starting from **3 raw macros**, the pipeline constructs **20 nutritional intelligence features**:

### Energy Estimation
```python
Calories_est  = Protein(g) × 4  +  Carbs(g) × 4  +  Fat(g) × 9   # Atwater factors
Total_macros_g = Protein(g) + Carbs(g) + Fat(g)
```

### Macro Composition Ratios
| Feature | Formula | Discriminative Power |
|---------|---------|---------------------|
| `Protein_pct` | `(Protein×4) / Calories × 100` | High — paleo, keto distinction |
| `Carbs_pct` | `(Carbs×4) / Calories × 100` | High — keto vs. vegan separation |
| `Fat_pct` | `(Fat×9) / Calories × 100` | **Highest** — keto signature |
| `Fat_carb_ratio` | `Fat / (Carbs + ε)` | **Highest** — top SHAP feature |
| `Protein_carb_ratio` | `Protein / (Carbs + ε)` | Medium — paleo indicator |
| `Protein_density` | `Protein / (Calories + ε) × 100` | Medium |
| `Macro_balance_score` | `1 − std(macro_pcts) / mean(macro_pcts)` | DASH identifier |

### Binary Diet Flags
```python
High_fat_flag     = 1 if Fat_pct > 50%      # Ketogenic signature
High_carb_flag    = 1 if Carbs_pct > 50%    # Vegan / DASH indicator
High_protein_flag = 1 if Protein_pct > 30%  # Paleo indicator
```

### Statistical Transforms
- **Z-scores** per macro (outlier-robust relative positioning)
- **Percentile ranks** per macro (non-parametric ordering)
- **Log1p transforms** for right-skewed macro distributions
- **Cuisine label encoding** (19 categories → ordinal integer)
- **Recipe name length** (proxy for recipe complexity)

---

## ◈ Models & Performance

### All Models Evaluated

| Category | Models |
|----------|--------|
| **Linear** | Logistic Regression, Ridge Classifier, SGD Classifier |
| **Distance** | K-Nearest Neighbours |
| **Tree** | Decision Tree, Random Forest, Extra Trees |
| **Gradient Boosting** | AdaBoost, GradientBoosting, HistGradientBoosting |
| **Advanced Boosting** | XGBoost ⚡, LightGBM ⚡, CatBoost ⚡ |
| **Ensemble** | Soft Voting, Hard Voting, Stacking (RF+XGB+CB → LR) |

> ⚡ GPU-accelerated with automatic CPU fallback

### Evaluation Metrics

Every model is assessed on **9 metrics**:

```
Accuracy  ·  Precision  ·  Recall  ·  F1 Macro  ·  F1 Weighted
ROC-AUC (OvR)  ·  Balanced Accuracy  ·  Matthews Correlation Coefficient  ·  Cohen's Kappa
```

### Evaluation Outputs
- Per-class confusion matrices (top 5 models)  
- Multi-class ROC curves (one-vs-rest per diet)  
- Precision-Recall curves per class  
- Interactive HTML leaderboard  
- Full classification reports with support counts  

---

## ◈ Hyperparameter Optimisation

### Optuna — Bayesian Search (TPE Sampler)

Applied to **XGBoost** and **LightGBM** independently.

**Search space:**
```python
n_estimators      : [100, 500]
learning_rate     : [0.01, 0.30]  (log scale)
max_depth         : [3, 10]
num_leaves        : [15, 127]     (LGB only)
subsample         : [0.50, 1.00]
colsample_bytree  : [0.50, 1.00]
reg_alpha         : [1e-5, 10.0]  (log scale)
reg_lambda        : [1e-5, 10.0]  (log scale)
```

- **30 trials** per model · 3-fold stratified CV · F1 Macro objective  
- All trial results saved to `outputs/tuning/`

### RandomizedSearchCV — Random Forest

```python
n_estimators      : [100, 200, 300, 500]
max_depth         : [None, 5, 10, 15, 20]
min_samples_split : [2, 5, 10]
min_samples_leaf  : [1, 2, 4]
max_features      : ['sqrt', 'log2', 0.5]
```
- 20 random iterations · 3-fold CV

---

## ◈ Explainable AI

### SHAP — TreeExplainer (LightGBM Tuned)

```
Global Explanations
├── Summary dot plot    — feature value vs. impact direction per class
├── Bar importance plot — mean |SHAP| across all predictions
└── Permutation vs SHAP comparison

Local Explanations (per-prediction)
└── Waterfall plots for one sample from each of 5 diet classes
    → Answers: "Why was this recipe classified as keto / vegan / etc.?"
```

**Key XAI Finding:**  
`Fat_carb_ratio` consistently ranks as the most discriminative feature — reflecting the fundamental nutritional difference between ketogenic (fat >> carbs) and vegan (carbs >> fat) dietary patterns. `Protein_pct` and `Carbs_pct` drive the Mediterranean–DASH boundary.

---

## ◈ Folder Structure

```
Diet_Classification_Enterprise_Pipeline.ipynb   ← Main notebook
README.md                                        ← This file

Health_Diet_AI_Project/
└── outputs/
    ├── eda/
    │   ├── missing_values_heatmap.png
    │   ├── class_distribution.png
    │   ├── cuisine_analysis.png
    │   ├── univariate_macronutrients.png
    │   ├── macro_pairplot.png
    │   ├── correlation_heatmaps.png
    │   ├── interactive_3d_scatter.html     ← Interactive
    │   ├── parallel_coordinates.html      ← Interactive
    │   ├── radar_chart.png
    │   └── data_quality_summary.csv
    │
    ├── features/
    │   ├── feature_selection_dashboard.png
    │   ├── feature_ranking.csv
    │   └── engineered_distributions.png
    │
    ├── models/
    │   ├── lgb_tuned.joblib
    │   ├── xgb_tuned.joblib
    │   ├── rf_tuned.joblib
    │   ├── cb_model.joblib
    │   ├── soft_voter.joblib
    │   ├── stacking.joblib
    │   ├── preprocessor.joblib
    │   ├── label_encoder.joblib
    │   ├── baseline_results.csv
    │   └── regularisation_study.png
    │
    ├── tuning/
    │   ├── xgb_optuna_trials.csv
    │   └── tuned_model_results.csv
    │
    ├── evaluation/
    │   ├── master_leaderboard.csv
    │   ├── confusion_matrices.png
    │   ├── roc_curves.png
    │   ├── pr_curves.png
    │   └── leaderboard_interactive.html   ← Interactive
    │
    ├── xai/
    │   ├── shap_summary_dot.png
    │   ├── shap_bar_importance.png
    │   ├── shap_importance.csv
    │   ├── shap_waterfall_<diet>.png      ← One per class
    │   └── perm_vs_shap.png
    │
    ├── reports/
    │   ├── executive_report.txt
    │   └── technical_report.txt
    │
    └── pipeline.log                       ← Full run log

Diet_Classification_Project.zip            ← Complete bundle export
```

---

## ◈ Quick Start

### 1 — Dataset Setup

Place all six CSV files in a single directory:

```
your_data_directory/
├── All_Diets.csv
├── mediterranean.csv
├── dash.csv
├── vegan.csv
├── keto.csv
└── paleo.csv
```

### 2 — Configure the Data Path

In **Phase 0** of the notebook, set:

```python
CONFIG = {
    'data_dir' : '/path/to/your_data_directory',   # ← update this
    ...
}
```

### 3 — Run the Pipeline

Open `Diet_Classification_Enterprise_Pipeline.ipynb` and execute all cells from top to bottom.  
All outputs are written automatically to `Health_Diet_AI_Project/outputs/`.

### 4 — Inference on New Data

```python
import joblib, pandas as pd, numpy as np

# Load saved artifacts
preproc = joblib.load('outputs/models/preprocessor.joblib')
model   = joblib.load('outputs/models/lgb_tuned.joblib')
le      = joblib.load('outputs/models/label_encoder.joblib')

# New recipe — macros only needed
new_recipe = pd.DataFrame([{
    'Protein(g)': 45.0,
    'Carbs(g)':   12.0,
    'Fat(g)':     38.0,
}])

# Engineer features (same function as training)
new_fe     = engineer_features(new_recipe)
new_proc   = preproc.transform(new_fe[selected_features])
prediction = model.predict(new_proc)

print(f'Predicted diet type: {le.inverse_transform(prediction)[0]}')
# → 'keto'
```

---

## ◈ Orchestration Layer

The pipeline ships four reusable production classes:

```python
DataManager(data_dir, target, seed)
    .load(filename)          # Read CSV from path
    .validate()              # Assert schema + report quality
    .split(test, val)        # Stratified 3-way split → X_train, X_val, X_test

FeatureEngineer()
    .fit_transform(df)       # Apply all 20 engineering transformations
    .get_feature_names()     # Return feature list for ColumnTransformer

ModelTrainer(seed)
    .add(name, model)        # Register any sklearn-compatible estimator
    .train_all(X_tr, y_tr,
               X_te, y_te)  # Fit all, score all, return leaderboard DataFrame

Evaluator()
    .full_report(y_true,
                 y_pred,
                 y_proba)    # Return all 9 metrics + print classification report

ExplainabilityManager(model, feature_names)
    .fit(X)                  # Compute TreeExplainer SHAP values
    .global_importance()     # Return mean |SHAP| Series sorted by impact
```

These classes are designed to be imported directly into downstream applications, FastAPI inference servers, or batch scoring pipelines.

---

## ◈ Visualisations

The pipeline generates **18 publication-quality figures** and **3 interactive HTML charts**:

| Output | Type | Phase |
|--------|------|-------|
| Macronutrient distributions × 5 diets | PNG | 3 |
| Class distribution bar + pie | PNG | 3 |
| Cuisine × Diet heatmap | PNG | 3 |
| Macro scatter matrix | PNG | 3 |
| Per-dataset correlation heatmaps | PNG | 3 |
| **3D macro scatter** (Plotly) | **HTML** | 3 |
| **Parallel coordinate plot** (Plotly) | **HTML** | 3 |
| Normalised radar chart | PNG | 3 |
| Engineered feature distributions | PNG | 4 |
| Feature selection dashboard (4-panel) | PNG | 5 |
| Regularisation study curves | PNG | 10 |
| Baseline model comparison | PNG | 7 |
| Confusion matrices (top 5 models) | PNG | 12 |
| Multi-class ROC curves | PNG | 12 |
| Precision-Recall curves | PNG | 12 |
| **Interactive model leaderboard** (Plotly) | **HTML** | 15 |
| SHAP summary dot plot | PNG | 13 |
| SHAP bar importance | PNG | 13 |
| SHAP waterfall plots (per class) | PNG | 13 |
| Permutation vs SHAP comparison | PNG | 13 |

---

## ◈ Future Roadmap

```
v2.0  ─  Recipe Name NLP
          TF-IDF + sentence-BERT embeddings from recipe names
          Fusion of textual + nutritional feature spaces

v2.1  ─  Tabular Neural Networks
          PyTorch TabNet / Entity Embedding for categorical features

v2.2  ─  Serving Layer
          FastAPI REST endpoint wrapping saved .joblib artifacts
          /predict  →  { protein, carbs, fat }  →  { diet_type, confidence }

v2.3  ─  Production Monitoring
          Evidently AI drift detection on macro distributions
          Weekly re-training trigger on drift threshold breach

v2.4  ─  Multi-Label Classification
          A recipe can satisfy multiple diets simultaneously
          Binary Relevance and Label Powerset approaches

v3.0  ─  Nutritional Scoring API
          Health-score regression alongside classification
          Personalised dietary fit scoring by user metabolic profile
```

---

## ◈ Contributing

Contributions are welcome. Please follow this workflow:

```bash
# Fork & clone
git clone https://github.com/your-username/diet-classification-pipeline.git
cd diet-classification-pipeline

# Create a feature branch
git checkout -b feature/your-feature-name

# Make changes, then run the notebook end-to-end to verify no regressions

# Commit with conventional commits
git commit -m "feat: add TabNet baseline model to Phase 8"

# Push and open a Pull Request
git push origin feature/your-feature-name
```

**Areas where contributions are especially valued:**
- Additional model architectures (TabNet, NODE, FT-Transformer)
- Cuisine-level sub-classification experiments
- Docker containerisation of the inference pipeline
- Unit tests for `DataManager` and `FeatureEngineer` classes

---

<div align="center">

---

**Built with rigour. Explained with clarity. Deployed with confidence.**

[![Python](https://img.shields.io/badge/Made%20with-Python-3776AB?style=flat-square&logo=python)](https://python.org)
[![sklearn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![XGBoost](https://img.shields.io/badge/XGBoost-FF6600?style=flat-square)](https://xgboost.readthedocs.io)
[![SHAP](https://img.shields.io/badge/SHAP-Explainability-8B5CF6?style=flat-square)](https://shap.readthedocs.io)
[![Optuna](https://img.shields.io/badge/Optuna-Bayesian%20Tuning-2196F3?style=flat-square)](https://optuna.org)

*MIT License · Open for research and commercial use*

</div>
