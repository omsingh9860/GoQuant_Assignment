📈 GoQuant Implied Volatility Forecasting Model

1. Project Overview
This project presents a complete solution for the GoQuant Implied Volatility Prediction assignment. The objective is to forecast the 10-second-ahead implied volatility of ETH using high-frequency order book data.

The final model is a sophisticated stacking ensemble that combines the predictions of LightGBM and CatBoost models. It was rigorously validated using a 5-fold time-series cross-validation, achieving a strong and reliable performance on the official Pearson Correlation Coefficient metric. This document details the methodology, assumptions, and instructions for reproducing the results.

2. 🛠️ Methodology
The project followed a structured, professional workflow designed for robustness and accuracy in a quantitative finance context.

2.1. Data Preparation & Cleaning
The raw, high-frequency data contained significant noise, including duplicate entries for the same timestamp, which caused both MemoryError and ValueError during development.

Solution: A critical pre-processing step was implemented to de-duplicate the data. All datasets were grouped by their timestamp, and the values for each second were aggregated (using the mean). This created a clean, uniform 1-second resolution time-series, which was essential for the model's stability and performance.

2.2. Feature Engineering
The model's predictive power was driven by a rich set of engineered features designed to capture different aspects of market behavior from the ETH order book.

Market Microstructure:

Weighted Average Price (WAP): Used as the primary price indicator, as it is more robust to noise than the mid-price.

Order Book Imbalance (OBI): A powerful feature calculated from the total bid and ask volumes to quantify buying vs. selling pressure.

Price Spreads: The bid-ask spread and differences between deeper levels of the order book were used to measure market liquidity.

Time-Series & Momentum:

Realized Volatility: Calculated as the rolling standard deviation of log returns over a 100-second window.

Price Momentum: The 10-second difference in WAP (wap_diff_10).

Cyclical Time Features: The hour of the day was encoded using sine and cosine transformations, allowing the model to learn intraday patterns more effectively.

2.3. Modeling Strategy: A Stacking Ensemble
A sophisticated ensemble strategy was employed to maximize accuracy.

Validation: A 5-fold TimeSeriesSplit was used for all cross-validation. This methodology guarantees the model is always trained on past data and validated on future data, preventing lookahead bias.

Base Models: Two powerful gradient boosting models, LightGBM and CatBoost, were trained on the engineered features.

Stacking: Instead of a simple average, a stacking technique was used. The out-of-fold predictions from both base models were used as features to train a final "meta-model" (a simple LightGBM regressor). This meta-model learned the optimal way to combine the base model predictions, resulting in a superior final forecast.

3. ⚙️ How to Run the Code
The entire pipeline is contained within a single Python script or Jupyter Notebook.

Setup:

Place the script in a directory with the competition data. The expected folder structure is:

your_project_folder/
├── your_script.py
├── submission.csv
└── train/
    └── ETH.csv
└── test/
    └── ETH.csv

Install the required libraries (see section 4).

Configuration:

Open the script and update the file paths in the Configuration section at the top to match the location of your data files.

Execution:

Run the script from top to bottom. It will automatically perform all steps and save a submission_final.csv file in the specified output path.

4. 📚 Libraries Used
This project requires the following Python libraries. You can install them using pip:

pip install pandas numpy lightgbm catboost scikit-learn scipy matplotlib

pandas & numpy: For data manipulation.

scikit-learn: For the TimeSeriesSplit cross-validation.

lightgbm & catboost: The two gradient boosting libraries for the base models.

scipy: For calculating the Pearson Correlation Coefficient.

matplotlib: For plotting results.

5. 📝 Assumptions Made
The provided order book data for ETH is the primary source of predictive power.

The statistical patterns learned from the historical data will generalize to the hidden test set.

Aggregating duplicate timestamps by taking the mean is a reasonable approach that preserves the central tendency of the data for that second.

6. 🏆 Final Performance
The stacking methodology proved highly effective, delivering a significant performance boost over the individual models.

Model

Cross-Validation Pearson Correlation

LightGBM

0.614861

CatBoost

0.646609

Stacked Ensemble

0.665573 ✨

The final model demonstrates a strong, reliable correlation with the true implied volatility, making it a valuable and robust forecasting tool.
