# 🔬 Biohub Cell Tracking — Ultra-Deep Phase-Wise Pipeline

> **CZI Biohub · Zebrafish Embryo · 3D+Time Cell Detection, Tracking & Division Linking**
> *Kaggle GPU (Tesla T4) · Classical + Deep-Learning · 86,054-row Submission*

---

## 📌 Competition Brief

| | |
|---|---|
| **Host** | Chan Zuckerberg Initiative (CZI) Biohub |
| **Task** | Detect cell centroids in 3D+time fluorescence microscopy volumes and link them into tracking graphs with division events |
| **Data format** | Zarr v3 `(T, Z, Y, X)` — typically `(100, 64, 256, 256)` uint16 |
| **Voxel scale** | z = 1.625 µm · y = x = 0.40625 µm (anisotropic) |
| **Annotation** | Sparse `.geff` graphs (nodes = centroids, edges = temporal links) |
| **Metric** | Bipartite node-match within **7.0 µm** physical gate |

---

## 🗂️ Pipeline Phases

```
Phase 0  ─── Environment & GPU bring-up
Phase 1  ─── Data contract & CONFIG dataclass
Phase 2  ─── Metadata EDA (geometry, embryo splits, estimated density)
Phase 3  ─── Ground-truth graph EDA (displacements, dt, divisions)
Phase 4  ─── Classical baseline detector (Otsu/percentile + NMS + refinement)
Phase 5  ─── GPU deep-learning detector (TinyUNet3D heatmap regression)
Phase 6  ─── Calibration sweep · SHAP · LIME · Permutation importance
Phase 7  ─── Tracking & linking (Hungarian + division 2nd-pass)
Phase 8  ─── Local graph proxy scoring (sparse recall, ρ, edge Jaccard)
Phase 9  ─── Submission build & 6-point schema audit
Phase 10 ─── Model comparison · failure modes · enhancement roadmap
Phase 11 ─── Export & zip of all artefacts
```

---

## ⚡ Key Results

| Metric | Value |
|---|---|
| GPU | Tesla T4 · 15.64 GB · torch 2.10.0+cu128 |
| Active detector | Classical (Otsu/percentile + NMS + centroid refinement) |
| Test samples processed | 4 samples · 400 frames total |
| **Total submission rows** | **86,054** (44,020 nodes + 42,034 edges) |
| Edge / node ratio | 0.955 — 95.5% of cells successfully linked |
| Per-sample time | 13.5 – 21.9 s per 100-frame volume |
| Schema audit | ✅ 6 / 6 checks PASS |
| Figures produced | 43 figures across phases 2–10 |
| CSV artefacts | 16 tables |

---

## 🏗️ Classical Detector Architecture

```
Raw volume (64 × 256 × 256, uint16)
        │
        ▼
Block-mean XY pooling  ─── XY_DS = 4  →  (64 × 64 × 64) working grid
        │
        ▼
Gaussian smooth  ─── σ = (1.0, 1.0, 1.0)
        │
        ▼
Adaptive threshold  ─── θ = max(Otsu, p50 + 0.18 × (p99.8 − p50))
        │
        ▼
3D local-maximum peak finding  ─── min_distance = 3 voxels
        │
        ▼
Centre-of-mass centroid refinement  ─── raw-volume patch (±6 yx, ±2 z)
        │
        ▼
Physical NMS  ─── 4.0 µm radius
        │
        ▼
Border filter  ─── 2% XY margin removal
        │
        ▼
Detected centroids  (Z, Y, X) in original voxel coords
```

---

## 🧠 Deep-Learning Detector (TinyUNet3D)

```
Input patch (1 × 32 × 128 × 128) normalised float32
        │
   Enc-1 (8 ch) ──────────────────────────── skip-1
        │ MaxPool3d(2)
   Enc-2 (16 ch) ─────────────────────────── skip-2
        │ MaxPool3d(2)
   Enc-3 / Bottleneck (32 ch)
        │ ConvTranspose3d(2)
   Dec-2 (16 ch) ← concat(skip-2)
        │ ConvTranspose3d(2)
   Dec-1 (8 ch)  ← concat(skip-1)
        │
   Output Conv3d(1) → sigmoid heatmap
        │
   Peak extraction  ─── thresh = 0.45, min_dist = 3
        │
   Centroids in working-grid coords → back-project to original voxels
```

*Training: BCEWithLogitsLoss · Adam lr=1e-3 · max 6 epochs · 10-min hard budget*

---

## 🔗 Tracking Pipeline

```
Frame t  ──── centroids (N × 3)
Frame t+1 ── centroids (M × 3)
        │
        ▼
cdist in physical µm space  (scale × voxel coords)
        │
        ▼
Hungarian assignment  ─── gate = 10.5 µm
        │
        ▼
Division 2nd-pass:
  Unmatched next-frame detections
  → check dist to nearest existing track  ≤ 9.0 µm (parent gate)
  → check sister-cell dist               ≤ 9.0 µm
  → optional midpoint cross-check        ≤ 6.0 µm
        │
        ▼
Stream  →  node rows (t, z, y, x)  +  edge rows (source_id, target_id)
```

---

## 📁 Output Artefacts

```
outputs/
├── figures/          # 43 × PNG (phases 2–10)
├── tables/           # 16 × CSV
│   ├── phase0_gpu_diagnostics.csv
│   ├── phase1_config_snapshot.csv
│   ├── phase2_train_sample_metadata.csv
│   ├── phase4_effective_tracking_geometry.csv
│   ├── phase9_submission_audit.csv
│   ├── phase10_model_comparison_table.csv
│   ├── phase10_failure_mode_analysis.csv
│   ├── phase10_enhancement_recommendations.csv
│   └── ... (16 total)
├── models/           # tiny_unet3d_heatmap.pt (when DL trained)
├── submission/
│   └── submission.csv   ← 86,054 rows, 6/6 audit checks PASS
└── logs/
    └── experiment_log.csv
all_outputs.zip       ← complete artefact bundle for download
```

---

## 🚀 How to Run on Kaggle

```
1. Upload this .ipynb to Kaggle → Code → New Notebook → File → Import
2. Right sidebar → Accelerator → GPU T4 x2
3. Right sidebar → Data → Add dataset → biohub-cell-tracking-during-development
4. Session options → Persistence → Files
5. Run All  (estimated runtime: 25–40 min with full training data)
```

---

## 🔧 Key Hyperparameters

| Parameter | Default | Effect |
|---|---|---|
| `THRESH_REL` (α) | 0.18 | **Most sensitive** — controls detection threshold |
| `MIN_PEAK_DIST` | 3 | Minimum peak separation on working grid |
| `NMS_RADIUS_UM` | 4.0 µm | Physical deduplication radius |
| `MAX_LINK_DIST_UM` | 10.5 µm | Hungarian linking gate |
| `DIV_PARENT_DIST_UM` | 9.0 µm | Division parent–daughter gate |
| `DL_PEAK_THRESH` | 0.45 | Sigmoid heatmap acceptance threshold |
| `XY_DS` | 4 | XY block-mean factor (near-isotropic grid) |

---

## 📈 Top Enhancement Priorities

| # | Enhancement | Gain | Effort |
|---|---|---|---|
| P1 | Larger DL model (UNet++ / 3D ResUNet) | +5–15% recall | Medium |
| P2 | Per-sample adaptive THRESH_REL | +3–8% recall | **Low** |
| P3 | Kalman motion prediction for linking | +2–5% precision | Medium |
| P4 | GNN embryo-aware linking | +3–6% edge Jaccard | High |
| P5 | Test-time augmentation (TTA) | +1–3% recall | **Low** |

---

## 📄 Report

A full **68-section Word report** (`biohub_cell_tracking_report.docx`) documents every phase, figure, CSV table, failure mode, and recommendation — following the same professional format as the companion Gummy Formulation Study report. Blank figures include written justifications explaining the root cause and expected content in a full data run.

---

*Prepared by: R&D Analytics Team · July 2026 · Internal / Confidential*
