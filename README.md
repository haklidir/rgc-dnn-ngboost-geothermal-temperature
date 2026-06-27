# Synthetic Hydrogeochemical Data and Machine Learning for Geothermal Reservoir Temperature Estimation in Western Anatolia

Code and notebooks for the study:

> **Advancing Geothermal Reservoir Temperature Prediction in The Western Anatolia (Türkiye) Using Synthetic Hydrogeochemical Data from Rank–Gaussian Copulas**
>
> Füsun S. Tut Haklıdır¹, Mehmet Haklıdır²
> ¹ Istanbul Bilgi University, Department of Energy Systems Engineering, Eyüp İstanbul, Türkiye
> ² TÜBİTAK BİLGEM, Gebze Kocaeli, Türkiye
>
> *Geothermics*, 2026.

The repository implements:

- A rank-Gauss copula (RGC) generator for routine hydrogeochemical variables.
- A DNN ensemble pre-trained on RGC-synthetic data and fine-tuned on real samples.
- NGBoost regressors trained on the real subset and on RGC-augmented data.

The three notebooks reproduce the marginal and multivariate diagnostics in Tables 1, 2a, 2b and the model performance comparison reported in Table 3 of the paper.

## Repository layout

```
.
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
│
├── data/
│   ├── README_data.md
│   ├── training_dataset.csv          # not included; see Data section
│   ├── testing_dataset.csv           # not included; see Data section
│   └── synthetic_rgc_train_only.csv  # produced by notebook 01
│
├── notebooks/
│   ├── 01_RGC_synthetic_data_generation.ipynb
│   ├── 02_DNN_RGC_pretraining_and_finetuning.ipynb
│   └── 03_NGBoost_real_vs_RGC_augmented.ipynb
│
├── src/
│   └── __init__.py                   # placeholder for future shared utilities
│
└── outputs/                          # populated when the notebooks run
    ├── rgc_outputs/
    ├── dnn_outputs/
    └── ngb_outputs/
```

## Requirements

Tested with Python 3.10 and the package versions listed in `requirements.txt`.

```bash
pip install -r requirements.txt
```

NGBoost and TensorFlow are pulled in here; both should install cleanly on standard CPU systems. A GPU is not required.

## Data

The two real CSV files (`training_dataset.csv`, `testing_dataset.csv`) are not distributed with this repository. Access is subject to the data-sharing conditions stated in the paper and can be requested from the corresponding author.

Expected columns (in this order):

```
pH, EC (microS/cm), K (mg/l), Na (mg/l), Boron (mg/l), SiO2 (mg/l), Cl (mg/l), Reservoir temperature (°C)
```

See `data/README_data.md` for further details.

## How to run

The notebooks are intended to be executed in order:

1. `01_RGC_synthetic_data_generation.ipynb`
   Fits the RGC model on the training set and writes `synthetic_rgc_train_only.csv` plus diagnostic plots to `rgc_outputs/`.

2. `02_DNN_RGC_pretraining_and_finetuning.ipynb`
   Pre-trains a DNN ensemble on the synthetic file produced in step 1 and fine-tunes on the real training set. Test-set predictions are written to `dnn_outputs/`.

3. `03_NGBoost_real_vs_RGC_augmented.ipynb`
   Fits two NGBoost regressors (real-only and RGC-augmented) and writes the comparison to `ngb_outputs/`.

Each notebook is a single code cell. To launch:

```bash
jupyter notebook notebooks/
```

or run non-interactively:

```bash
jupyter nbconvert --to notebook --execute notebooks/01_RGC_synthetic_data_generation.ipynb
```

## Reproducibility

All notebooks set `GLOBAL_SEED = 42`. The DNN ensemble uses per-member seed offsets so that variability is controlled at the ensemble level. The standard scaler used in notebook 02 is fit on the real training set only; the synthetic data and the test set are transformed but never used to fit the scaler. RGC is fit on the training set only, so the synthetic file carries no information from the test set.

## Citation

If you use this code, please cite:

```bibtex
@article{TutHaklidir2026RGC,
  author  = {Tut Haklıdır, Füsun S. and Haklıdır, Mehmet},
  title   = {Advancing Geothermal Reservoir Temperature Prediction in The Western
             Anatolia (Türkiye) Using Synthetic Hydrogeochemical Data from
             Rank--Gaussian Copulas},
  journal = {Geothermics},
  year    = {2026}
}
```

(Volume, pages and DOI will be added once the article appears in the journal issue.)

## License

MIT License. See `LICENSE` for the full text.

## Contact

For data requests and questions about the paper, please contact the corresponding author:
Füsun S. Tut Haklıdır, `fusun.tut@bilgi.edu.tr`

For repository issues, please open a GitHub issue.
