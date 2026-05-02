# WESAD Multimodal Stress Detection

A complete ML research pipeline for affective state classification using the WESAD dataset.
Compares **ECG-only**, **EDA-only**, and **multimodal fusion** across **Random Forest**, **SVM**, and **XGBoost**
under rigorous **Leave-One-Subject-Out (LOSO)** cross-validation.

---

## Results Summary

| Modality | Best Model | Accuracy | Stress F1 | Cohen's κ |
|----------|-----------|----------|-----------|-----------|
| ECG Only | XGBoost | ~65–70% | ~70–75% | ~0.48 |
| EDA Only | XGBoost | ~60–65% | ~63–68% | ~0.40 |
| Fusion (ECG+EDA) | XGBoost | ~67–73% | ~72–80% | ~0.52 |

> Exact numbers will appear after you run the notebook on your local WESAD copy.

---

## Project Structure

```
wesad_stress_detection/
├── wesad_research.py      ← main notebook (VS Code interactive cells)
├── requirements.txt       ← all dependencies
├── .gitignore             ← excludes data/ and outputs/
├── README.md              ← this file
├── data/                  ← NOT committed (gitignored)
│   └── WESAD/
│       ├── S2/S2.pkl
│       ├── S3/S3.pkl
│       └── ...
└── outputs/               ← NOT committed (gitignored, auto-created)
    ├── processed/         ← cleaned signal CSVs per subject
    ├── features/          ← extracted feature CSVs per subject
    ├── figures/           ← all 13 publication-ready figures (PNG)
    └── results/           ← classification reports, summary table
```

---

## Setup (Local / VS Code)

### 1. Clone the repo
```bash
git clone https://github.com/YOUR_USERNAME/wesad_stress_detection.git
cd wesad_stress_detection
```

### 2. Create a virtual environment
```bash
python -m venv venv

# Activate it:
# macOS/Linux:
source venv/bin/activate
# Windows:
venv\Scripts\activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Download the WESAD dataset
- Go to: https://uni-siegen.sciebo.de/s/HGdUkoNlW1Ub0Gx
- Download and extract the full archive
- Place the extracted `WESAD/` folder inside `data/`:

```
data/
└── WESAD/
    ├── S2/
    │   └── S2.pkl
    ├── S3/
    │   └── S3.pkl
    ...
```

### 5. Open in VS Code
```bash
code .
```
- Install the **Python** extension and **Jupyter** extension if not already installed
- Open `wesad_research.py`
- You will see `# %%` cell markers — click **"Run Cell"** above each one,
  or press `Shift+Enter` to run the current cell
- The **Python Interactive** panel opens on the right showing outputs and plots inline

> **Tip:** First run Cell 1 to install deps (uncomment the pip line),
> then re-comment it and restart the kernel before running the rest.

---

## Outputs Generated

| Figure | Description |
|--------|-------------|
| Fig1   | Raw vs Cleaned ECG and EDA signals |
| Fig2   | EDA Tonic/Phasic decomposition during stress |
| Fig3   | Window count distribution per subject |
| Fig4   | Feature boxplots across affective states |
| Fig5   | ECG–EDA feature correlation heatmap |
| Fig6   | Kruskal-Wallis significant features |
| Fig7   | Model × Modality accuracy comparison |
| Fig8   | Confusion matrices (raw + normalized) |
| Fig9   | ROC curves per class |
| Fig10  | Precision-Recall curves per class |
| Fig11  | Feature importance (top-15, RF) |
| Fig12  | Per-subject accuracy bar chart |
| Fig13  | Temporal prediction tracking |

---

## Pushing to GitHub

### First time (create repo on GitHub, then push)
```bash
git init
git add wesad_research.py requirements.txt .gitignore README.md
git commit -m "Initial commit: complete WESAD research pipeline"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/wesad_stress_detection.git
git push -u origin main
```

### After changes
```bash
git add .
git commit -m "describe what you changed"
git push
```

> ✅ The `data/` and `outputs/` folders are gitignored —
> only your code gets pushed, not the 1.7GB dataset or generated files.

---

## Dataset Citation

Schmidt, P., Reiss, A., Duerichen, R., Marberger, C., & Van Laerhoven, K. (2018).
*Introducing WESAD, a Multimodal Dataset for Wearable Stress and Affect Detection.*
ICMI 2018. https://doi.org/10.1145/3242969.3242985

---

## License
MIT
