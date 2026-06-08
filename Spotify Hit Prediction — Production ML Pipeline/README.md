
<div align="center">

```
███████╗██████╗  ██████╗ ████████╗██╗███████╗██╗   ██╗
██╔════╝██╔══██╗██╔═══██╗╚══██╔══╝██║██╔════╝╚██╗ ██╔╝
███████╗██████╔╝██║   ██║   ██║   ██║█████╗   ╚████╔╝ 
╚════██║██╔═══╝ ██║   ██║   ██║   ██║██╔══╝    ╚██╔╝  
███████║██║     ╚██████╔╝   ██║   ██║██║        ██║   
╚══════╝╚═╝      ╚═════╝    ╚═╝   ╚═╝╚═╝        ╚═╝   
         HIT PREDICTION  ·  END-TO-END ML PIPELINE
```

# 🎵 Spotify Hit Prediction — Production ML Pipeline

**Predict chart-topping songs from audio DNA using 13 classifiers, Optuna tuning, GPU acceleration, SHAP explainability & stacking ensembles**

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Colab](https://img.shields.io/badge/Google%20Colab-GPU%20Ready-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white)](https://colab.research.google.com)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.0%2B-FF6600?style=for-the-badge)](https://xgboost.readthedocs.io)
[![SHAP](https://img.shields.io/badge/SHAP-XAI-8A2BE2?style=for-the-badge)](https://shap.readthedocs.io)
[![Optuna](https://img.shields.io/badge/Optuna-Hypertuning-00B4D8?style=for-the-badge)](https://optuna.org)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

---

> *What makes a song go viral? This pipeline decodes Spotify's audio DNA — from danceability to valence — using a production-grade ML stack that mirrors real industry forecasting systems.*

</div>

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Dataset](#-dataset)
- [Pipeline Architecture](#-pipeline-architecture)
- [Phase-by-Phase Breakdown](#-phase-by-phase-breakdown)
- [Models & Performance](#-models--performance)
- [Feature Engineering](#-feature-engineering)
- [Explainability (XAI)](#-explainability-xai)
- [Quick Start](#-quick-start)
- [Output Artefacts](#-output-artefacts)
- [Technical Stack](#-technical-stack)
- [Project Structure](#-project-structure)
- [Design Decisions](#-design-decisions)
- [Known Limitations](#-known-limitations)

---

## 🧭 Overview

This end-to-end notebook predicts whether a Spotify track will be a **hit** (top 20% by popularity score) using only its **audio and descriptive features** — no streaming count data, no social signals. It is designed to:

- Run **zero-change** on Google Colab with GPU or CPU
- Auto-detect whether you upload **1 CSV** or the **high/low split** (two files)
- Produce **industry-grade outputs** — models, plots, classification reports, SHAP artefacts — all zipped for download
- Push to **GitHub without errors** (no widget state metadata)

---

## 📊 Dataset

| File | Rows | Popularity Range | Label |
|------|------|-----------------|-------|
| `high_popularity_spotify_data.csv` | 1,686 | 68 – 100 | `High` |
| `low_popularity_spotify_data.csv` | 3,145 | 11 – 68 | `Low` |
| **Combined** | **4,831** | **11 – 100** | — |

### Audio Features Used

| Feature | Description | Range |
|---------|-------------|-------|
| `energy` | Intensity & activity | 0–1 |
| `danceability` | Rhythmic suitability for dancing | 0–1 |
| `valence` | Musical positiveness (happy ↔ sad) | 0–1 |
| `loudness` | Overall track loudness (dB) | −60–0 |
| `tempo` | Beats per minute (BPM) | 0–250 |
| `acousticness` | Confidence: acoustic vs electric | 0–1 |
| `speechiness` | Presence of spoken words | 0–1 |
| `instrumentalness` | Vocal-free likelihood | 0–1 |
| `liveness` | Audience presence probability | 0–1 |
| `duration_ms` | Track length in milliseconds | — |
| `key` | Musical key (0–11 pitch class) | 0–11 |
| `mode` | Major (1) or Minor (0) | 0–1 |
| `time_signature` | Beats per measure | 3–7 |

### Descriptive Features Used

| Feature | Description |
|---------|-------------|
| `track_artist` | Performing artist(s) |
| `playlist_genre` | Playlist genre (pop, rock, etc.) |
| `playlist_subgenre` | Playlist subgenre |
| `track_album_release_date` | Album release date |

---

## 🏗️ Pipeline Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                    SPOTIFY HIT PREDICTION PIPELINE                   │
├──────────┬──────────┬──────────┬──────────┬──────────┬─────────────┤
│  Phase 0 │  Phase 1 │  Phase 2 │  Phase 3 │  Phase 4 │   Phase 5   │
│  Setup   │  Data    │  EDA     │  Feature │ Baseline │   Advanced  │
│  & Libs  │  Loading │  +Stats  │  Engg.   │  Models  │   Tuning    │
│          │  (auto)  │          │  +SMOTE  │ (13 clf) │  (Optuna)   │
├──────────┴──────────┴──────────┴──────────┴──────────┴─────────────┤
│  Phase 6          │  Phase 7               │  Phase 8              │
│  Ensemble &       │  Full Comparison       │  XAI: SHAP +          │
│  Stacking         │  + All Visuals         │  Permutation + Force  │
├───────────────────┴────────────────────────┴───────────────────────┤
│  Phase 9 — Save All Outputs + ZIP Download                         │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🔬 Phase-by-Phase Breakdown

### ⚙️ Phase 0 — Environment Setup
- Installs all required packages silently (`shap`, `xgboost`, `lightgbm`, `catboost`, `optuna`, `imbalanced-learn`, `plotly`, `kaleido`)
- Detects GPU availability and sets device flags automatically
- Creates structured output directories for clean artefact organisation

### 📂 Phase 1 — Data Loading
- **Auto-detects file layout:** accepts `high_*` + `low_*` files or any single merged CSV
- Normalises column name differences between files
- Validates `track_popularity` column and deduplicates on `track_id`
- Prints data summary and label distribution

### 🔍 Phase 2 — Advanced EDA (7 visual panels)
| Plot | What It Shows |
|------|--------------|
| Popularity Distribution | Histogram + KDE + median/mean lines |
| Feature Distributions | KDE overlay: High vs Low per feature |
| Boxplots | Feature spread by popularity label |
| Correlation Heatmap | Pearson r matrix for all audio features |
| Statistical Tests | T-test + Mann-Whitney U with significance flags |
| Temporal Analysis | Popularity trend & track count by release year |
| Genre Analysis | Mean popularity by genre and top-15 subgenres |
| Pairplot | Key feature interactions coloured by label |

### 🛠️ Phase 3 — Advanced Feature Engineering
**18 new features engineered from raw audio signals:**

| Category | Features |
|----------|----------|
| Audio Interactions | `energy_dance_product`, `mood_score`, `vocal_score`, `acoustic_energy_diff`, `positivity_gap` |
| Normalised | `loudness_norm`, `duration_min` |
| Binary Flags | `is_short_track`, `is_long_track` |
| Temporal | `Release_Year`, `Release_Month`, `Release_DayOfWeek`, `Track_Age_Days` |
| Frequency Encoding | `playlist_genre_freq`, `playlist_subgenre_freq` |
| Target Encoding | `artist_hit_rate` (Bayesian smoothed, k=20) |
| Log Transforms | Applied to all features with \|skew\| > 1.0 |
| OHE | `playlist_genre`, `playlist_subgenre`, `mode`, `tempo_bin`, `key` |

**Class balancing:** `SMOTETomek` — combines SMOTE oversampling with Tomek link removal for cleaner decision boundaries.

### 🤖 Phase 4 — Baseline Framework (13 Classifiers)
All trained on balanced data with identical evaluation:

```
Logistic Regression  ·  Random Forest  ·  Gradient Boosting
XGBoost  ·  LightGBM  ·  CatBoost  ·  Extra Trees
AdaBoost  ·  Bagging(DT)  ·  SVM(RBF)  ·  KNN
Gaussian NB  ·  LDA
```

Each model outputs: `Accuracy`, `F1`, `Precision`, `Recall`, `AUC-ROC`, `MCC`, `Log Loss`, `Brier Score`, `Train time`.

### 🚀 Phase 5 — Advanced Tuning (Optuna + GPU)
- **4 models tuned:** XGBoost, LightGBM, Random Forest, CatBoost
- **60 trials each** via TPE Sampler (total: 240 trials)
- **GPU acceleration** enabled automatically when CUDA is available
- Hyperparameter search covers: `learning_rate`, `max_depth`, `subsample`, `colsample_bytree`, regularisation (`reg_alpha`, `reg_lambda`, `l2_leaf_reg`), `num_leaves`, `min_child_samples`, and more

### 🏗️ Phase 6 — Ensembles
- **Soft Voting:** XGBoost + LightGBM + Random Forest + CatBoost
- **Stacking (5 base estimators):** XGBoost + LightGBM + Random Forest + CatBoost + Extra Trees → Logistic Regression meta-learner
- 5-fold cross-validation for stacking with `passthrough=False`

### 📊 Phase 7 — Full Comparison (6 visual panels)
- Metric heatmap (all models × all metrics)
- Grouped bar chart (top-12 models, 4 metrics)
- Confusion matrices (top-6 models)
- ROC curves (all models, solid = tuned/ensemble)
- Precision-Recall curves (all models)
- Calibration curves (top-4 models)

### 🔬 Phase 8 — XAI & Explainability
- **Built-in importance** for all tree models (side-by-side)
- **SHAP Beeswarm** — global feature impact with direction
- **SHAP Bar** — ranked mean \|SHAP\| values
- **SHAP Dependence plots** — top-4 features with auto-interaction
- **SHAP Waterfall** — 3 prediction archetypes (correct hit, missed hit, false positive)
- **Permutation importance** — model-agnostic, 20 rounds, AUC-ROC metric
- **Interactive SHAP Force Plot** — saved as standalone HTML

### 💾 Phase 9 — Outputs & Download
- All visuals (PNG), models (PKL), reports (TXT), SHAP artefacts (NPY/CSV/HTML)
- Optuna best params saved as JSON per model
- All packaged in a timestamped `.zip` and auto-downloaded

---

## 🤖 Models & Performance

### Metrics Collected Per Model

| Metric | What It Measures |
|--------|-----------------|
| **Accuracy** | Overall correct predictions |
| **F1 Score** | Harmonic mean of precision & recall |
| **Precision** | Of predicted hits, how many are real hits |
| **Recall** | Of real hits, how many we captured |
| **AUC-ROC** | Discrimination ability across all thresholds |
| **MCC** | Matthews Correlation Coefficient — balanced metric for class imbalance |
| **Log Loss** | Probabilistic prediction quality (lower = better) |
| **Brier Score** | Mean squared error of probability predictions (lower = better) |

> **Primary ranking metric:** AUC-ROC (robust to class imbalance and threshold-independent)

---

## 🛠️ Feature Engineering

### Why These Features?

**`mood_score = valence × energy`**  
Captures the emotional intensity of a track — high-valence, high-energy songs (dance anthems) behave very differently from high-valence, low-energy songs (acoustic folk).

**`vocal_score = speechiness − instrumentalness`**  
Quantifies how "vocal-forward" a track is. Radio hits cluster at high vocal scores.

**`artist_hit_rate`** (Bayesian smoothed)  
Encodes historical artist success without leaking future data. Smoothing factor k=20 prevents overfitting on artists with only 1–2 tracks.

**`Track_Age_Days`**  
Recent tracks dominate streaming charts. This feature captures recency bias in the popularity scoring algorithm.

**`loudness_norm = (loudness + 60) / 60`**  
Rescales the naturally negative dB range to [0,1] for stable gradient descent.

---

## 🔮 Explainability (XAI)

### Reading SHAP Plots

**Beeswarm Plot:**
- Each dot = one track in the test set
- X-axis = SHAP value (positive → pushes toward "Hit")
- Colour = feature value (red = high, blue = low)
- Features sorted by mean \|SHAP\| (most important at top)

**Dependence Plot:**
- X-axis = raw feature value for each track
- Y-axis = SHAP impact on prediction
- Colour = automatically selected interacting feature
- Reveals non-linear thresholds (e.g. "loudness above −5dB reliably predicts hits")

**Waterfall Plot:**
- Shows how each feature additively moves a single prediction from base rate to final probability
- Three archetypes analysed: correct hit, missed hit (FN), false positive (FP)

---

## 🚀 Quick Start

### Option A — Google Colab (Recommended)

```
1. Open Google Colab
2. Upload  Spotify_Hit_Prediction_Pipeline.ipynb
3. Runtime → Change Runtime Type → T4 GPU
4. Runtime → Run All
5. When prompted, upload:
      high_popularity_spotify_data.csv
      low_popularity_spotify_data.csv
6. Wait for pipeline to complete (~15–25 min on T4)
7. ZIP auto-downloads to your machine
```

### Option B — Single File Upload

Upload any CSV with a `track_popularity` column and the notebook auto-creates a median-split binary target.

### Option C — Local Jupyter

```bash
pip install jupyter xgboost lightgbm catboost shap optuna imbalanced-learn plotly kaleido
jupyter notebook Spotify_Hit_Prediction_Pipeline.ipynb
# Replace colab file upload cells with: pd.read_csv("your_file.csv")
```

---

## 📦 Output Artefacts

After the pipeline completes, `spotify_ml_outputs_<timestamp>.zip` contains:

```
spotify_outputs/
├── eda/
│   ├── 01_popularity_distribution.png
│   ├── 02_feature_distributions.png
│   ├── 03_boxplots.png
│   ├── 04_correlation_heatmap.png
│   ├── 05_temporal_analysis.png
│   ├── 06_genre_analysis.png
│   ├── 07_pairplot.png
│   └── statistical_tests.csv
│
├── features/
│   └── 01_skewness.png
│
├── models/
│   ├── best_model.pkl              ← Serialised best model
│   ├── scaler.pkl                  ← RobustScaler for inference
│   ├── features.json               ← Ordered feature list
│   ├── baseline_leaderboard.csv
│   ├── tuned_leaderboard.csv
│   ├── 01_baseline_comparison.png
│   ├── 02_roc_pr_baseline.png
│   ├── optuna_xgboost_best_params.json
│   ├── optuna_lightgbm_best_params.json
│   ├── optuna_randomforest_best_params.json
│   └── optuna_catboost_best_params.json
│
├── xai/
│   ├── 01_builtin_importance.png
│   ├── 02_shap_beeswarm.png
│   ├── 03_shap_bar.png
│   ├── 04_shap_dependence.png
│   ├── 05_shap_waterfall.png
│   ├── 06_permutation_importance.png
│   ├── shap_force_plot.html        ← Interactive (open in browser)
│   ├── shap_values.npy
│   └── shap_values.csv
│
├── comparison/
│   ├── master_leaderboard.csv      ← All models ranked
│   ├── 01_heatmap_leaderboard.png
│   ├── 02_grouped_bar.png
│   ├── 03_confusion_matrices.png
│   ├── 04_roc_pr_all.png
│   └── 05_calibration.png
│
└── reports/
    └── <ModelName>_report.txt      ← Full classification report per model
```

---

## 🧰 Technical Stack

| Category | Library | Version |
|----------|---------|---------|
| Data | `pandas`, `numpy` | Latest |
| Visualisation | `matplotlib`, `seaborn`, `plotly` | Latest |
| ML Framework | `scikit-learn` | 1.3+ |
| Boosting | `xgboost`, `lightgbm`, `catboost` | 2.0 / 4.0 / 1.2+ |
| Imbalanced | `imbalanced-learn` | 0.11+ |
| Tuning | `optuna` | 3.4+ |
| Explainability | `shap` | 0.44+ |
| Statistics | `scipy` | Latest |
| Serialisation | `pickle`, `json` | stdlib |

---

## 📁 Project Structure

```
spotify-hit-prediction/
├── Spotify_Hit_Prediction_Pipeline.ipynb  ← Main notebook
├── README.md                              ← This file
├── high_popularity_spotify_data.csv       ← Upload to Colab
├── low_popularity_spotify_data.csv        ← Upload to Colab
└── spotify_ml_outputs_<timestamp>.zip     ← Generated output
```

---

## 🎯 Design Decisions

| Decision | Rationale |
|----------|-----------|
| **RobustScaler over StandardScaler** | Spotify audio features (loudness, tempo) have outliers; RobustScaler uses IQR and is less distorted |
| **SMOTETomek over plain SMOTE** | Generates synthetic minority samples AND removes borderline majority samples for cleaner decision boundaries |
| **Extreme-quantile target (top 20%)** | A strict hit/not-hit threshold produces a harder, more meaningful classification problem than median split |
| **Bayesian smoothing for artist encoding** | Prevents artist hit-rate from being dominated by artists with 1–2 tracks in the dataset |
| **AUC-ROC as primary metric** | Threshold-independent; appropriate for imbalanced binary classification |
| **No `metadata.widgets` key** | Notebook is written programmatically with clean metadata — no Jupyter widget state that causes GitHub rendering errors |
| **Modular `evaluate_model()` function** | Single evaluation interface for all models ensures fair, reproducible comparison |

---

## ⚠️ Known Limitations

- **Popularity score is time-dependent** — A song's popularity changes as streams accumulate. The dataset is a snapshot; predictions reflect that moment in time.
- **No lyrics or social data** — The model uses audio features only. Social virality (TikTok, sync placements) is not captured.
- **Artist leakage risk** — `artist_hit_rate` is computed on the full dataset before split. For strict evaluation, this should be computed only on training data.
- **Small dataset** — ~4,800 tracks is small for deep learning; tree-based models are well-suited but generalisation should be validated on held-out recent data.
- **Genre proxy** — `playlist_genre` reflects editorial playlist curation, not the intrinsic genre of the track.

---

## 📜 License

```
MIT License — free to use, modify, and distribute with attribution.
```

---

<div align="center">

**Built with 🎵 for the intersection of music and machine learning**

*Audio features courtesy of Spotify Web API · Pipeline design inspired by production forecasting systems*

[![Made with Python](https://img.shields.io/badge/Made%20with-Python-1f425f.svg?style=flat-square)](https://www.python.org/)
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

</div>
