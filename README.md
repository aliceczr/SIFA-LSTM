# SIFA-LSTM

Time-series forecasting of **Acquired Syphilis** notifications in Brazil using **LSTM** neural networks, based on public SINAN/DATASUS data.

## Overview

This project builds daily and weekly forecasts of confirmed syphilis cases (ICD-10 `A53.9`) from Brazil's national disease notification system (SINAN). The full pipeline — data extraction, preprocessing, model training, and evaluation — is implemented in a single notebook: [`LSTM_SIFA26.ipynb`](LSTM_SIFA26.ipynb).

## Repository structure

```
SIFA-LSTM/
├── LSTM_SIFA26.ipynb   # Main notebook: extraction, preprocessing, models, evaluation
├── data/               # Raw SINAN CSVs, one file per notification year (2014–2024)
└── README.md
```

## Data

- Source: SINAN/DATASUS, agravo `A539` (Acquired Syphilis).
- Only confirmed cases (`CLASSI_FIN == 1`) are kept; records from 2024 are dropped (incomplete year).
- Cases are aggregated by diagnosis date into **daily** and **weekly** (3-week moving average) series.
- Data is downloaded automatically inside the notebook via the GitHub API, no manual upload needed.

## Models

**Daily model** — input window of 20 days, forecasts 7 days ahead.
`LSTM(50) → Dropout(0.2) → Dense(7)`, RMSprop optimizer, custom RMSE loss, 150 epochs, batch size 16.

**Weekly model** — input window of 3 weeks, forecasts 1 week ahead.
`LSTM(256) → Dropout(0.2) → Dense(1)`, same optimizer/loss setup.

Both use fixed random seeds (42) for reproducibility, with a chronological train/validation/test split (no shuffling).

## Evaluation

For each model: RMSE, normalized RMSE (NRMSE), and Pearson correlation between predicted and actual values (per forecast horizon for the daily model).

## Running it

**Google Colab** (recommended): open `LSTM_SIFA26.ipynb` directly from GitHub in Colab and run cells top to bottom.

**Locally:**
```bash
git clone https://github.com/aliceczr/SIFA-LSTM.git
cd SIFA-LSTM
pip install pandas tensorflow keras scikit-learn numpy matplotlib seaborn requests
jupyter notebook LSTM_SIFA26.ipynb
```

## Author

[**aliceczr**](https://github.com/aliceczr)

## License

No license specified.
