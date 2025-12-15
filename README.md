# SI 670 Applied Machine Learning Final Project: Fantasy Football Quantile Predictions

**University of Michigan**

---

## 🚀 Quick Start — Run These Notebooks

| Notebook | Purpose | Run Order |
|----------|---------|-----------|
| **`SI670_Final_Project_Model_v4.ipynb`** | Core model training & evaluation | **Run First** |
| **`SI670_Final_Project_Fantasy_v2.ipynb`** | ESPN Fantasy integration & roster predictions | **Run Second** |

> **Note:** Each notebook is self-contained. Run all cells from top to bottom.

---

## Project Overview

This project applies **quantile regression** to predict fantasy football scores with uncertainty quantification. Rather than providing a single point estimate, the model predicts floor (10th percentile), median (50th percentile), and ceiling (90th percentile) outcomes for each player—enabling fantasy managers to make risk-informed lineup decisions.

## Motivation

Standard fantasy projections provide a single number (e.g., "14.5 points"), but two players can project the same with very different risk profiles. Quantile regression addresses this by explicitly modeling the distribution of outcomes, answering questions like:
- What's the safe floor for this player?
- What's their upside potential?
- How consistent vs. volatile are they?

## Data

- **Source:** NFL play-by-play data via `nfl_data_py` (2015-2024 seasons)
- **Scope:** ~500,000+ fantasy-relevant plays across 10 seasons
- **Target:** Weekly fantasy points (Half-PPR scoring)
- **Integration:** ESPN Fantasy API for real roster predictions

## Features

The model uses sophisticated feature engineering including:
- **Time-series features:** Lagged points, rolling averages, cumulative averages
- **Volatility features:** Rolling std, MAE, deviation from average
- **Workload features:** Play share, target share
- **Opponent features:** Defensive EPA allowed (rolling 5-week)
- **Boom probability:** Spike rate (% of recent games ≥20 points)

## Model

- **Algorithm:** Histogram Gradient Boosting Quantile Regressor
- **Quantiles:** 0.10 (floor), 0.50 (median), 0.90 (ceiling)
- **Hyperparameters:** max_depth=8, learning_rate=0.01, max_iter=2000
- **Training:** Chronological 90/10 split (no data leakage)

## Repository Structure

```
├── SI670_Final_Project_Model_v4.ipynb    # ⭐ PRIMARY: Model training & evaluation
├── SI670_Final_Project_Fantasy_v2.ipynb  # ⭐ PRIMARY: ESPN integration & predictions
├── SI670_Final_Project_EDA.ipynb         # Exploratory data analysis
├── SI670_Final_Project_Model.ipynb       # Initial model development (archived)
├── SI670_Final_Project_Model_v2.ipynb    # Model iteration (archived)
├── SI670_Final_Project_Model_v3.ipynb    # Model iteration (archived)
├── SI670_Final_Project_Fantasy.ipynb     # Initial fantasy integration (archived)
└── README.md
```

## Results

**Model Performance (Model_v4):**
- **MAE (Median):** 2.47 fantasy points
- **80% Prediction Interval Coverage:** 76.89%
- **Pinball Loss (10th quantile):** 0.4358

**Benchmark Comparison:**
| Model | Test MAE | Test R² | PICP |
|-------|----------|---------|------|
| Quantile HGBR (Ours) | 2.47 | 0.70 | 76.89% |
| Decision Tree | 2.58 | 0.71 | N/A |
| Ridge Regression | 2.94 | 0.65 | N/A |

## Requirements

```bash
pip install nfl_data_py espn_api scikit-learn pandas numpy matplotlib seaborn
```

## Usage

### Option 1: Model Only (Model_v4)
1. Open `SI670_Final_Project_Model_v4.ipynb`
2. Run all cells
3. View model evaluation, feature importance, and quantile curves

### Option 2: Full Fantasy Integration (Fantasy_v2)
1. Open `SI670_Final_Project_Fantasy_v2.ipynb`
2. (Optional) Update ESPN credentials for your league
3. Run all cells
4. View floor/median/ceiling predictions for your roster

## Authors

- Kyle Ciarkowski
- Arjun Mayur
