# Electricity Demand Forecasting with a Heterogeneous Ensemble

This university machine-learning project forecasts Spain's next-hour electricity demand from hourly energy and weather data covering 2015 to 2018.

The main objective was not only to obtain a low test error, but to build a forecasting workflow that respects time order and avoids target leakage.

**Project status:** complete  
**Main notebook:** [`electricity_demand_forecasting.ipynb`](electricity_demand_forecasting.ipynb)

## Authors

This was a four-person UPF project by Didac Bustamante, Nil Castaño, Taiki Causton, and Joan Garcia Almenara.

## Method

- Cleaned and merged hourly electricity and weather data.
- Used a chronological 70/15/15 train-validation-test split.
- Compared every model with a previous-day seasonal-naive forecast.
- Created lag features at 1, 24, and 168 hours.
- Applied `shift(1)` before 24-hour rolling statistics.
- Added calendar, holiday, previous-day price, and nonlinear temperature features.
- Used `TimeSeriesSplit` for hyperparameter searches where tuning was required.
- Compared regularised linear models, KNN, Linear SVR, Random Forest, Gradient Boosting, XGBoost, MLP, and ensemble forecasts.

## Main result

| Model | Test RMSE MWh | Test MAE MWh | Test MAPE | Test R-squared |
|---|---:|---:|---:|---:|
| Seasonal naive | 3,627.7 | - | - | - |
| Random Forest | **534.5** | **352.6** | **1.22%** | **0.9864** |
| Optimised ensemble | 535.6 | 360.4 | 1.25% | 0.9863 |
| XGBoost | 547.8 | 374.6 | 1.29% | 0.9857 |

Random Forest had the lowest test RMSE. The optimised ensemble was almost identical but did not improve on the strongest individual model, which is an important limitation rather than a result to hide.

## Repository structure

```text
.
├── electricity_demand_forecasting.ipynb
├── data
│   └── README.md
├── .gitignore
└── requirements.txt
```

## Data

The notebook expects `energy_dataset.csv` and `weather_features.csv` inside `data/raw/`. The data are not redistributed in this repository. See [`data/README.md`](data/README.md) for the expected files and source.

## Run locally

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open `electricity_demand_forecasting.ipynb` and run the cells from top to bottom.

The full run includes cross-validated searches for several models and may take time on a laptop. The committed notebook retains the verified outputs so the analysis can be reviewed directly on GitHub.

## Limitations

- Weather observations from five large cities are only a proxy for national conditions.
- Random Forest and XGBoost show a larger train-validation gap than the regularised linear models.
- The test period is one historical segment, so results may change under later structural shifts.
- The project forecasts one hour ahead and does not evaluate longer horizons.
