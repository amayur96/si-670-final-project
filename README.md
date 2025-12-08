# SI 670 Final Project: Fantasy Football Quantile Predictions

**Course:** SI 670 - Applied Machine Learning  
**University of Michigan School of Information**

## Project Overview

This project applies **quantile regression** to predict fantasy football scores with uncertainty quantification. Rather than providing a single point estimate, the model predicts floor (10th percentile), median (50th percentile), and ceiling (90th percentile) outcomes for each player—enabling fantasy managers to make risk-informed lineup decisions.

## Motivation

Standard fantasy projections provide a single number (e.g., "14.5 points"), but two players can project the same with very different risk profiles. Quantile regression addresses this by explicitly modeling the distribution of outcomes, answering questions like:
- What's the safe floor for this player?
- What's their upside potential?
- How consistent vs. volatile are they?

## Data

- **Source:** NFL play-by-play data via `nfl_data_py` (2022-2025 seasons)
- **Scope:** ~150,000 plays filtered to ~107,000 fantasy-relevant observations
- **Target:** Weekly fantasy points (Half-PPR scoring)
- **Integration:** ESPN Fantasy API for real roster predictions

## Features

| Feature | Description |
|---------|-------------|
| Y_lag_1 | Previous week's fantasy points |
| Y_roll_avg_3 | 3-week rolling average |
| Y_cum_avg | Season/career average |
| Y_std_3 | 3-week volatility (standard deviation) |
| Y_min_3 | Recent floor (minimum of last 3 weeks) |
| Y_max_3 | Recent ceiling (maximum of last 3 weeks) |
| Position | One-hot encoded (QB, RB, WR/TE, Other) |

## Model

- **Algorithm:** Histogram Gradient Boosting Quantile Regressor
- **Quantiles:** 0.10, 0.50, 0.90
- **Training:** Time-series split (train on weeks before prediction week)

## Repository Structure

```
├── SI670_Final_Project_EDA.ipynb      # Exploratory data analysis
├── SI670_Final_Project_Model.ipynb    # Initial model development
├── SI670_Final_Project_Model_v2.ipynb # Extended model with visualizations
├── SI670_Final_Project_Fantasy.ipynb  # ESPN integration & predictions
└── README.md
```

## Results

For Week 13 of the 2025 NFL season:
- **90% prediction interval coverage** (9/10 actual scores within predicted range)
- Model successfully captures fantasy football's inherent volatility
- Limitations: Wide prediction intervals due to limited pre-game features

## Requirements

```
pip install nfl_data_py espn_api scikit-learn pandas numpy matplotlib
```

## Usage

1. Update ESPN credentials in `SI670_Final_Project_Fantasy.ipynb`
2. Run all cells to train model and generate predictions
3. View floor/median/ceiling predictions for your roster

## Authors

SI 670 Applied Machine Learning - Final Project
