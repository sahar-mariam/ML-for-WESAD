# WESAD Stress Detection — Minimal Calibration Study

> *"Do stress detectors need to know you? A cross-modal coherence feature study on WESAD."*

A complete ML research pipeline investigating whether subject-specific calibration improves physiological stress detection when cross-modal autonomic coherence features are used.

---

## Key Finding

**82.01% LOSO accuracy with zero calibration data** — the highest reported subject-independent result on WESAD chest sensors. Adding calibration data (up to 25 minutes) provides no meaningful improvement and may degrade performance (McNemar's χ² = 65.61, p < 0.001).

---

## Results Summary

| Model | K=0 (no calib) | K=1 (30s) | K=5 (2.5m) | K=20 (10m) | K=50 (25m) | Best κ |
|-------|---------------|-----------|------------|------------|------------|--------|
| **XGBoost** | **82.01%** | 82.12% | 82.09% | 82.05% | 79.04% | **0.696** |
| RandomForest | 80.39% | 80.52% | 80.40% | 80.36% | 77.56% | 0.667 |
| SVM | 77.82% | 77.91% | 77.80% | 77.63% | 75.23% | 0.626 |

> K = number of 30-second windows given to the model from the test subject before evaluation.
> K=0 = pure Leave-One-Subject-Out (LOSO). No subject data seen during training.

---

## What Makes This Different

Most WESAD papers extract ECG features and EDA features **separately** and concatenate them. This work additionally computes **cross-modal coherence features** — the physiological coupling between the two signals.

Under stress, HRV drops and EDA rises simultaneously. That anti-correlated co-activation is a direct signature of autonomic nervous system activation that neither signal captures alone.

### Novel Features (6 cross-modal)

| Feature | What it captures |
|---------|-----------------|
| ECG-EDA Pearson correlation | Linear coupling between signals |
| ECG-EDA Spearman correlation | Non-linear coupling |
| LF Band Coherence (0.04–0.15 Hz) | Baroreflex-EDA coupling |
| HF Band Coherence (0.15–0.40 Hz) | Respiratory-EDA coupling |
| RMSSD / Tonic SCL ratio | Autonomic balance index |
| HF Power / SCR Peaks ratio | Parasympathetic vs sympathetic ratio |

---

## Project Structure

```
wesad_stress_detection/
├── wesad_calibration.ipynb    ← main notebook (run top to bottom)
├── requirements.txt           ← all dependencies
├── .gitignore                 ← excludes data/ and outputs/
├── README.md                  ← this file
├── data/                      ← NOT committed (gitignored)
│   └── WESAD/
│       ├── S2/S2.pkl
│       ├── S3/S3.pkl
│       └── ... (S4–S17, S12 absent)
└── outputs/                   ← NOT committed (gitignored, auto-created)
    ├── processed/             ← cleaned signal CSVs per subject
    ├── features/              ← extracted feature CSVs per subject
    ├── figures/               ← all publication-ready figures (PNG)
    └── results/               ← classification reports, calibration results
```

---

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/YOUR_USERNAME/wesad_stress_detection.git
cd wesad_stress_detection
```

### 2. Create a virtual environment
```bash
python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
```

### 3. Install dependencies
```bash
python -m pip install -r requirements.txt
```

### 4. Download WESAD
- Go to: https://uni-siegen.sciebo.de/s/HGdUkoNlW1Ub0Gx
- Download and extract
- Place the `WESAD/` folder inside `data/`:

```
data/
└── WESAD/
    ├── S2/S2.pkl
    ├── S3/S3.pkl
    └── ...
```

### 5. Register the Jupyter kernel
```bash
python -m pip install ipykernel
python -m ipykernel install --user --name=wesad_venv --display-name "Python (wesad_venv)"
```

### 6. Launch Jupyter
```bash
python -m jupyter notebook
```

Open `wesad_calibration.ipynb`, switch kernel to **Python (wesad_venv)**, edit `BASE_DIR` in Cell 3, then run cells top to bottom.

---

## Notebook Structure

| Cell | What it does | Speed |
|------|-------------|-------|
| 1 | Title and overview | — |
| 2 | Install dependencies (run once) | — |
| 3 | Imports and configuration — **edit BASE_DIR here** | Fast |
| 4 | Signal cleaning (skips existing) | Medium |
| 5 | Figure 1: Preprocessing QC plot | Fast |
| 6 | Figure 2: EDA decomposition during stress | Fast |
| 7 | Feature extraction — HRV + EDA + cross-modal | **Slow (20–40 min)** |
| 8 | Load master feature matrix | Fast |
| 9 | Add temporal lag-1 features | Fast |
| 10 | Figures 3–5: EDA plots | Fast |
| 11 | Figure 6: Kruskal-Wallis significance tests | Fast |
| 12 | Feature preparation + per-subject normalization | Fast |
| 13 | Model definitions | Fast |
| 14 | Calibration LOSO loop — main experiment | **Slowest (30–90 min)** |
| 15 | Results summary table | Fast |
| 16 | Figure 7: Calibration curve (main result) | Fast |
| 17 | Figure 8: Per-subject accuracy comparison | Medium |
| 18 | Figure 9: Confusion matrices | Fast |
| 19 | Figure 10: ROC curves | Fast |
| 20 | Figure 11: Feature importance | Medium |
| 21 | McNemar's significance test | Fast |
| 22 | Figure 12: Prior work comparison | Fast |
| 23 | Export all reports and final summary | Fast |

> **Tip:** Cells 4 and 7 skip automatically if outputs already exist. Safe to re-run the full notebook.

---

## Figures Generated

| Figure | Description |
|--------|-------------|
| Fig 1 | Raw vs cleaned ECG and EDA signals |
| Fig 2 | EDA tonic/phasic decomposition during stress |
| Fig 3 | Window count per subject per condition |
| Fig 4 | HRV and EDA feature distributions across states |
| Fig 5 | Cross-modal coherence features across states |
| Fig 6 | Kruskal-Wallis discriminative feature ranking |
| **Fig 7** | **Calibration curve — main result** |
| Fig 8 | Per-subject accuracy: K=0 vs best K |
| Fig 9 | Confusion matrices: no calibration vs calibration |
| Fig 10 | ROC curves: no calibration vs calibration |
| Fig 11 | Feature importance (top 25, color-coded by type) |
| Fig 12 | Prior work comparison bar chart |

---

## Comparison with Prior Work

| Study | Split | Accuracy |
|-------|-------|----------|
| Schmidt et al. 2018 | Subject-dependent | 93.0% |
| Siirtola 2019 | Subject-dependent | 90.2% |
| Bobade & Vani 2020 | Subject-dependent | 88.5% |
| Priya et al. 2020 | Subject-dependent | 85.3% |
| Can et al. 2019 | LOSO | 67.5% |
| **Proposed (XGBoost, K=0)** | **LOSO** | **82.01%** |

Subject-dependent splits inflate accuracy by 11–15 percentage points on WESAD by allowing the model to learn individual physiological patterns during training.

---

## Pushing to GitHub

```bash
git add wesad_calibration.ipynb requirements.txt .gitignore README.md
git commit -m "Initial commit: WESAD calibration study"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/wesad_stress_detection.git
git push -u origin main
```

After changes:
```bash
# In Jupyter: Kernel → Restart & Clear Output (keeps file size small)
git add wesad_calibration.ipynb
git commit -m "describe change"
git push
```


---

## Dependencies

```
neurokit2==0.2.7
xgboost==1.6.2
scikit-learn==1.3.2
pandas
numpy
matplotlib
seaborn
scipy
tqdm
statsmodels
jupyter
ipykernel
```

---

## Dataset Citation

Schmidt, P., Reiss, A., Duerichen, R., Marberger, C., & Van Laerhoven, K. (2018).
*Introducing WESAD, a Multimodal Dataset for Wearable Stress and Affect Detection.*
ICMI 2018. https://doi.org/10.1145/3242969.3242985

---
