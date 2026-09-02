# ML-Based Prediction of Polymer Glass Transition Temperature (Tg)

A machine-learning project for predicting polymer **glass transition temperature (Tg)** from SMILES-derived molecular descriptors and polymer class information.

This repository is a cleaned, reproducible portfolio version of a final project completed for **CL653 — Applications of AI and ML for Chemical Engineering**.

## Project overview

Glass transition temperature is an important thermal property that affects polymer processing, flexibility, stiffness, and service temperature. This project evaluates whether structure-based features can provide a useful baseline estimate of Tg before experimental validation.

The workflow covers:

- data cleaning and preprocessing
- structural feature extraction from SMILES
- molecular descriptor generation using **RDKit**
- grouped train-test splitting by SMILES to avoid direct structural overlap
- categorical preprocessing using one-hot encoding
- comparison of **Linear Regression, Random Forest, and XGBoost**
- evaluation using **MAE, RMSE, R², feature importance, parity plots, and residual analysis**

## Dataset

The project uses the Kaggle dataset **“Extra Dataset with SMILES, Tg, PID, Polymer Class”** by *linyeping*:

https://www.kaggle.com/datasets/linyeping/extra-dataset-with-smilestgpidpolimers-class

The third-party dataset is not redistributed in this repository. After downloading it, place the cleaned file here:

```text
data/TgSS_enriched_cleaned.csv
```

The portfolio preprocessing retains **7,207 polymer records** across **198 polymer classes**. The grouped split produces **5,768 training samples** and **1,439 test samples**, with zero identical SMILES shared between the two sets.

## Features

The model uses polymer class together with structure-derived features including:

- SMILES length
- counts of C, O, and N symbols
- molecular weight
- topological polar surface area (TPSA)
- rotatable bonds
- ring count
- hydrogen-bond donors and acceptors
- aromatic ring count

## Model performance

Performance on the grouped holdout test set:

| Model | Test MAE (°C) | Test RMSE (°C) | Test R² |
|---|---:|---:|---:|
| Dummy baseline | 93.59 | 110.85 | -0.002 |
| Linear Regression | 43.27 | 58.09 | 0.725 |
| Random Forest | 30.86 | 44.89 | 0.836 |
| **XGBoost** | **32.06** | **44.47** | **0.839** |

**Best overall test performance:** XGBoost with **R² ≈ 0.839** and **RMSE ≈ 44.47 °C**. Random Forest gives a slightly lower MAE and very similar overall performance.

The results indicate that non-linear ensemble models capture the relationship between the selected structural features and Tg more effectively than Linear Regression.

## Repository structure

```text
polymer-tg-prediction-ml/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
├── notebooks/
│   └── polymer_tg_prediction.ipynb
└── results/
    └── model_comparison.csv
```

## How to run

1. Clone the repository.
2. Create a Python environment.
3. Install the dependencies:

```bash
pip install -r requirements.txt
```

4. Download the Kaggle dataset and place `TgSS_enriched_cleaned.csv` inside the `data/` folder.
5. Open and run:

```text
notebooks/polymer_tg_prediction.ipynb
```

## Tools and libraries

**Python · pandas · NumPy · scikit-learn · XGBoost · RDKit · Matplotlib · Jupyter**

## Limitations and future work

This is a structure-based baseline rather than a complete formulation-property model. Polymer Tg can also depend on molecular-weight distribution, additives, curing conditions, composition ratios, processing history, and other formulation variables not available in this dataset.

Potential improvements include repeated grouped cross-validation, broader molecular representations, hyperparameter optimization, uncertainty estimation, and inclusion of formulation and process variables.

## References

1. Linyeping. *Extra Dataset with SMILES, Tg, PID, Polymer Class*. Kaggle.
2. Tao, L.; Varshney, V.; Li, Y. *Benchmarking Machine Learning Models for Polymer Informatics: An Example of Glass Transition Temperature*. Journal of Chemical Information and Modeling, 2021, 61(11), 5395–5413.
3. Nguyen, T.; Bavarian, M. *A Machine Learning Framework for Predicting the Glass Transition Temperature of Homopolymers*. Industrial & Engineering Chemistry Research, 2022, 61(34), 12690–12698.
4. Casanola-Martin, G. M.; Karuth, A.; Rasulev, B. *Machine learning analysis of a large set of homopolymers to predict glass transition temperatures*. Communications Chemistry, 2024, 7, 226.
