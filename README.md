# 2025 Asset Pricing — Quant & ML Portfolio

![Last Commit](https://img.shields.io/github/last-commit/hoseunggg/2025_asset_pricing)
![Repo Size](https://img.shields.io/github/repo-size/hoseunggg/2025_asset_pricing)
![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)

A collection of three self-contained projects at the intersection of **quantitative finance** and **machine learning** — cooperative game theory for portfolio attribution, factor-based ML asset pricing, and hyperparameter-search methodology.

## Table of Contents

- [Overview](#overview)
- [Projects](#projects)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Tech Stack](#tech-stack)
- [Reproducibility](#reproducibility)
- [Author](#author)

## Overview

Each project lives in its own folder with a dedicated `README.md` and `requirements.txt`, and can be explored independently. Projects 1 and 2 sit directly in the quantitative-finance domain; project 3 focuses on model-selection and hyperparameter-optimization methodology.

## Projects

### 1. Shapley Sector Portfolio &nbsp; `runnable ✅`

Treats the **11 S&P 500 sector ETFs as players in a cooperative game** and decomposes each sector's contribution to portfolio risk reduction and mean–variance utility using **Shapley values**.

- **Highlights** — game theory applied to empirical finance, Monte-Carlo approximation to avoid exponential ($2^{11}$) enumeration, rolling-window analysis of time-varying contributions
- **Stack** — NumPy · SciPy (SLSQP, linprog) · joblib · yfinance
- Full theory & derivations: [report/Trading_report.pdf](./shapley-sector-portfolio/report/Trading_report.pdf)

### 2. ML Asset Pricing &nbsp; `data required 🔑`

A scaled-down replication of Gu, Kelly & Xiu (2020), *"Empirical Asset Pricing via Machine Learning."* Predicts next-month cross-sectional returns from **17 factors** and backtests a **long–short (H-L) portfolio**.

- **Highlights** — factor engineering, chronological train/test split (no look-ahead), LASSO vs. XGBoost comparison, Sharpe / max-drawdown evaluation
- **Stack** — pandas · scikit-learn (LASSO) · XGBoost
- Requires CRSP monthly data (proprietary, not included — see the project README)

### 3. MNIST Hyperparameter Search &nbsp; `runnable ✅`

Benchmarks **four hyperparameter-search strategies** (Grid / Random / Bayesian / Hyperband) for a fully-connected network, with a `joblib`-parallel experiment pipeline.

- **Highlights** — model selection & hyperparameter optimization, parallel experiment design, regularization / dropout / overfitting analysis
- **Stack** — TensorFlow/Keras · Keras Tuner · joblib

## Project Structure

```
2025_asset_pricing/
├── shapley-sector-portfolio/
│   ├── shapley_sector_portfolio.ipynb
│   ├── report/Trading_report.pdf
│   ├── README.md
│   └── requirements.txt
├── ml-asset-pricing/
│   ├── ml_asset_pricing.ipynb
│   ├── README.md
│   └── requirements.txt
├── mnist-hp-search/
│   ├── mnist_hyperparameter_search.ipynb
│   ├── data/workflow.png
│   ├── results/
│   ├── README.md
│   └── requirements.txt
├── README.md
└── .gitignore
```

## Getting Started

```bash
git clone https://github.com/hoseunggg/2025_asset_pricing.git
cd 2025_asset_pricing/<project-folder>   # e.g. shapley-sector-portfolio
pip install -r requirements.txt
jupyter lab
```

Each notebook uses paths relative to **its own folder** (run it with that folder as the working directory). Projects 1 and 3 run out of the box (live yfinance data / built-in MNIST); project 2 needs CRSP data.

## Tech Stack

`Python` · `NumPy` · `pandas` · `SciPy` · `scikit-learn` · `XGBoost` · `TensorFlow/Keras` · `Keras Tuner` · `joblib` · `Matplotlib` · `yfinance`

## Reproducibility

- All projects fix a random seed (`SEED = 42`).
- External data dependencies (Yahoo Finance live download, CRSP) are documented in each project README. yfinance is live, so absolute figures may drift over time.
- After a first successful run, pin exact versions with `pip freeze > requirements.lock.txt`.

## Author

**Hoseung Kang** — [@hoseunggg](https://github.com/hoseunggg)
