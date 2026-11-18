# Demand Projection & Sales Forecasting

Forecasts 12 months of product demand from historical monthly sales and converts the forecast into a monthly inventory plan with safety stock.

**Tech stack:** Python, pandas, NumPy, statsmodels, scikit-learn, Matplotlib, Jupyter Notebook

## Problem
Stocking too little loses sales; stocking too much ties up cash. Both need a reliable demand forecast. This project builds that forecast and turns it into a stock decision.

## Data
72 months (Jan 2019 – Dec 2024) of **simulated** monthly unit sales for one product, built with a steady growth trend, a festive-season peak (Oct–Nov), a mid-year dip, and random noise. The code works on any CSV with `date` and `units` columns.

## Approach
1. **EDA:** trend plot, average units by month, seasonal decomposition
2. **Train/test split:** train on 2019–2023, test on 2024 (time-based, no shuffling, to avoid data leakage)
3. **Models compared:** Naive, Seasonal Naive, Moving Average (3 months), Linear Regression (trend + month dummies), Holt-Winters
4. **Evaluation:** MAE, RMSE, MAPE on the 2024 test set
5. **Final forecast:** best model retrained on all 72 months, 2025 forecast with 95% range
6. **Demand projection:** safety stock = Z × σ × √(lead time) at a 95% service level (Z = 1.65, lead time = 1 month); target stock = forecast + safety stock

## Results (2024 test set)
| Model | MAE | RMSE | MAPE % |
|---|---|---|---|
| Holt-Winters | 31.68 | 36.04 | 1.78 |
| Linear Regression | 56.77 | 69.29 | 2.93 |
| Seasonal Naive | 152.00 | 158.18 | 8.22 |
| Naive | 296.25 | 315.43 | 16.70 |
| Moving Avg (3) | 347.08 | 375.99 | 20.08 |

- Best model: **Holt-Winters** (trend + multiplicative seasonality)
- 2025 projected demand: **about 24,100 units (about 8.7% above 2024)**, peaking in November
- Safety stock: about 54 units per month

## Files
| File | Description |
|---|---|
| `Demand_Projection_Sales_Forecasting.ipynb` | Full notebook with outputs |
| `sales_data.csv` | Simulated monthly sales data |
| `model_comparison.csv` | Error metrics for all models |
| `demand_plan_2025.csv` | 2025 forecast and monthly stock plan |
| `01_eda.png`, `02_decomposition.png`, `03_model_comparison.png`, `04_forecast_2025.png` | Charts |

## How to run
```bash
pip install pandas numpy matplotlib statsmodels scikit-learn jupyter
jupyter notebook Demand_Projection_Sales_Forecasting.ipynb
```

## Limitations and next steps
- Simulated data, so the accuracy is optimistic; real data will be noisier
- Single product, no price, promotion, or holiday-date variables
- Next: real dataset, promotions as features, SARIMA/Prophet, rolling cross-validation, per-product forecasts
