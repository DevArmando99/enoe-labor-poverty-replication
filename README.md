# ENOE Labor Poverty Replication

[Versión en español](README_ES.md)

Reproducible reconstruction and statistical validation of Mexico's labor poverty indicator using ENOE microdata for the available quarterly series from 2006T1 to 2026T1.

## Project overview

This repository contains the applied replication and validation study built on top of the `enoe-utilities` processing library. The goal is not only to reproduce the official labor poverty series, but to document an auditable workflow from raw ENOE inputs to the final quarterly indicator and to evaluate how closely the reconstructed values agree with the official series.

The study covers 80 available quarters between 2006T1 and 2026T1. The reconstructed series tracks the official indicator almost perfectly in terms of temporal dynamics, while the validation also identifies a small systematic negative bias and a larger discrepancy in the most recent analyzed periods.

## Main results

- 80 quarterly observations were reconstructed and compared with the official series.
- Correlation and R² are approximately 1, showing near-perfect tracking of the official dynamics.
- Mean difference (Calculated − Official): approximately **−0.0166 percentage points**.
- Mean absolute error (MAE): approximately **0.0166 percentage points**.
- Maximum absolute discrepancy: approximately **0.0869 percentage points**.
- TOST establishes statistical equivalence for the full series under a practical margin of **±0.05 percentage points**.
- Equivalence is also established through 2024T1.
- From 2024T2 onward, equivalence cannot be established under the same margin.
- OLS and HAC/Newey-West results support a stronger association between the increase in discrepancies and the recent-period component than with the level of labor poverty itself.

The recent-period result is descriptive and inferential within the analyzed sample; it does not imply that the discrepancy will necessarily continue increasing in future quarters.

## Workflow

```text
ENOE ZIP microdata
        ↓
Conversion and schema normalization
        ↓
Required-column and key validation
        ↓
SDEM + COE2 person-level merge
        ↓
Analytical labor-income dataset
        ↓
Household labor income and per-capita income
        ↓
Rural / urban extreme-poverty income line
        ↓
Weighted labor-poverty estimate
        ↓
Official-vs-reconstructed statistical validation
        ↓
Historical analytical Parquet for subsequent analysis
```

## Statistical validation

The validation sequence intentionally goes beyond correlation:

1. Official vs. reconstructed time-series comparison.
2. Correlation and linear fit.
3. Analysis of quarterly differences and error magnitude.
4. One-sample t-test for systematic mean bias.
5. Bland–Altman concordance analysis.
6. Temporal and labor-poverty-level exploratory segmentation.
7. OLS regression and HAC/Newey-West robust inference.
8. TOST equivalence testing for the complete series and two temporal segments.

A practical TOST equivalence margin of **±0.05 percentage points** is used as half of the one-decimal publication resolution commonly used to communicate the official indicator. This is an analytical criterion for this replication study and **not an official INEGI or CONEVAL equivalence threshold**.

## Repository structure

```text
.
├── README.md
├── README_ES.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── Replica_Indice_Pobreza_Laboral.ipynb
├── results/
│   └── serie_pobreza_laboral_2006T1_2026T1.csv
├── docs/
│   └── figures/
└── reproducibility/
    ├── README.md
    └── README_ES.md
```

The large raw and intermediate datasets are intentionally kept outside GitHub. See the reproducibility documentation for the expected package structure and reconstruction instructions.

## Relationship with `enoe-utilities`

This repository is the applied case study and statistical validation layer. The reusable data-processing functions are maintained separately in the `enoe-utilities` repository.

- `enoe-utilities`: reusable ENOE processing and labor-poverty utilities.
- `enoe-labor-poverty-replication`: applied reconstruction, validation, and documented results.

## Reproducibility package

The full reproducibility package is designed to be distributed separately because the ENOE raw and intermediate files are too large for normal GitHub versioning. It includes the original source inputs, converted Parquet files, merged datasets, analytical quarterly files, historical analytical Parquet, indicators, metadata, paradata, and validation outputs.

Before redistributing original ENOE source files, verify the current INEGI terms applicable to the microdata. If redistribution is not appropriate, keep a manifest of exact official source files and download locations instead.

See [`reproducibility/README.md`](reproducibility/README.md) for details.

## Environment

The analysis is written in Python and uses pandas, NumPy, PyArrow, SciPy, statsmodels, Matplotlib, Seaborn, openpyxl, and Jupyter.

Install the core analysis dependencies with:

```bash
pip install -r requirements.txt
```

The `enoe-utilities` package must also be available in the environment used to rerun the full pipeline.

## Scope

This repository closes the first stage of a broader project. The validated historical analytical dataset is intended to support subsequent exploratory and statistical analysis of labor poverty and its determinants.

## Disclaimer

This is an independent reproducibility and portfolio project. It is not an official INEGI or CONEVAL product, and the practical equivalence criteria used in the statistical validation should not be interpreted as institutional tolerances.
