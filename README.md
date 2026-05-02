# Inter-Model Disagreement in Kepler Exoplanet Classification: A Leakage-Aware Benchmark

**Andrii Mykuliak, Andrii Vovk**  
Lviv Polytechnic National University, Lviv, Ukraine

This repository contains the full analysis pipeline for the paper:

> *Inter-Model Disagreement in Kepler Exoplanet Classification: A Leakage-Aware Benchmark*  
> Submitted to *Astronomy and Computing* (Elsevier)

---

## Overview

A reproducible machine learning benchmark for tabular classification of Kepler Objects of Interest (KOI) dispositions — confirmed exoplanets versus false positives — using twelve physically motivated transit, stellar, and signal-quality features. The benchmark is leakage-aware with respect to explicit vetting-derived fields (`koi_fpflag_*`, `koi_score`), which are deliberately excluded.

The Inter-Model Disagreement Analysis (IMDA) framework is applied to 1979 unclassified CANDIDATE objects to characterise where and why five trained classifiers disagree.

---

## Repository Structure

```
IMDA/
├── IMDA.ipynb          # Full analysis pipeline
├── requirements.txt    # Python dependencies
└── README.md
```

The dataset (`cumulative.csv`) is not included in this repository. Download it directly from the NASA Exoplanet Archive:

> https://exoplanetarchive.ipac.caltech.edu  
> Table: Kepler Objects of Interest (KOI) Cumulative Table  
> Version used: January 2025

Place `cumulative.csv` in the same directory as `IMDA.ipynb` before running.

---

## Reproducibility

All results in the paper are obtained with:

- `RANDOM_STATE = 42`
- Stability analysis seeds: `[0, 7, 21, 42, 99]`
- Dataset: NASA KOI cumulative catalogue, January 2025 snapshot

---

## Requirements

Python 3.13. Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Running the Notebook

```bash
jupyter notebook IMDA.ipynb
```

Run all cells in order. The notebook will:

1. Load and preprocess `cumulative.csv`
2. Split data using `GroupShuffleSplit` by host star (`kepid`)
3. Train five classifiers (LR, DT, RF, XGBoost, SVM)
4. Evaluate benchmark performance (Table 2)
5. Run cross-seed stability analysis (Table 4)
6. Apply IMDA to 1979 unclassified candidates
7. Generate all figures and tables reported in the paper

---

## Models

Five classifiers with default hyperparameters:

| Model | Implementation |
|---|---|
| Logistic Regression | `sklearn.linear_model.LogisticRegression` |
| Decision Tree | `sklearn.tree.DecisionTreeClassifier` |
| Random Forest | `sklearn.ensemble.RandomForestClassifier` |
| XGBoost | `xgboost.XGBClassifier` |
| SVM | `sklearn.svm.SVC(probability=True)` |

---

## Statistical Test

DeLong test (XGBoost vs Random Forest) implemented via [MLstatkit](https://github.com/Brritany/MLstatkit), following Sun & Xu (2014), *IEEE Signal Processing Letters* 21(11), 1389–1393.

Result: z = 1.22, p = 0.222 — no statistically significant difference.

---

## Citation

If you use this code, please cite:

```
Mykuliak, A., Vovk, A. (2025). Inter-Model Disagreement in Kepler Exoplanet
Classification: A Leakage-Aware Benchmark. Manuscript submitted to
Astronomy and Computing. https://doi.org/10.5281/zenodo.19958364
```

> **Note:** Citation will be updated with volume/issue/DOI upon acceptance.

---

## License

MIT License