📊 Media Mix Modeling with XGBoost

A marketing analytics solution that optimizes media spend allocation across multiple channels using advanced feature engineering, causal inference, and machine learning.

🚀 Overview

This project implements Media Mix Modeling (MMM) using XGBoost with causal inference techniques to accurately measure the incremental impact of different media channels on revenue.

The pipeline leverages geometric adstock transformations, saturation curves, and two-stage causal modeling to address endogeneity and provide actionable insights for optimal budget allocation.

🏗️ Approach
🔧 1. Feature Engineering
📈 Adstock Modeling

Captures carryover effects of advertising using geometric adstock transformation:

adstock
[
𝑡
]
=
spend
[
𝑡
]
+
decay_rate
×
adstock
[
𝑡
−
1
]
adstock[t]=spend[t]+decay_rate×adstock[t−1]

Tests multiple decay rates: 0.1, 0.3, 0.5, 0.7, 0.9

Selects optimal decay rate based on model performance

📉 Saturation Curves

Models diminishing returns on spend:

saturation
=
log
⁡
(
1
+
adstock
)
saturation=log(1+adstock)

This captures the non-linear relationship between spend and its incremental impact.

⏳ Temporal Features

Cyclical encoding of seasonality using sine/cosine transformations

Week-of-year and month indicators for periodic effects

Time index to capture long-term trends

🔄 Interaction Effects

Promotional interactions: (promotion × media_spend)

Lag features: 1–3 week lags for media, email, and SMS campaigns

🧠 2. Two-Stage Causal Modeling
Stage 1: Mediator Model

Predicts Google spend using other media channels + temporal features

Addresses endogeneity in spend allocation decisions

Generates predicted values & residuals for causal inference

Stage 2: Revenue Model

Uses predicted Google spend + residuals from Stage 1

Incorporates:

All media channels

Control variables

Engineered features

Separates causal effects from correlational effects

🎯 3. Hyperparameter Optimization

Optuna-based tuning for each decay rate

50 trials per configuration with early stopping

Parameters tuned:

learning_rate

max_depth

lambda, alpha (regularization)

Sampling rates

📊 Model Selection & Validation

Model Selection:

Choose best decay rate + hyperparameter combination

Based on R² score on test set

Validation Strategy:

80/20 time-ordered train-test split

Cross-validation with time-series splits

Early stopping to prevent overfitting

📈 Model Evaluation
Metric	Purpose
R²	Explained variance
MAE	Mean Absolute Error
RMSE	Root Mean Squared Error
MAPE	Percentage-based error assessment
🛠️ Key Technical Features

✅ XGBoost Regressor with regularization
✅ Geometric Adstock for carryover effects
✅ Two-Stage Causal Modeling to handle endogeneity
✅ Optuna Hyperparameter Tuning
✅ Time Series Validation to avoid leakage
✅ Interactive Visualizations with Plotly

📊 Results

Optimal Decay Rate automatically selected

Feature Importance Rankings for media channels

Revenue Predictions with uncertainty bounds

Visual Comparison of actual vs. predicted performanceMedia Mix Modeling with XGBoost
A marketing analytics solution that optimizes media spend allocation across multiple channels using advanced feature engineering and causal inference techniques.
Approach
1. Feature Engineering
Adstock Modeling

Implements geometric adstock transformation to capture carryover effects of advertising
Tests multiple decay rates (0.1, 0.3, 0.5, 0.7, 0.9) to find optimal carryover period
Formula: adstock[t] = spend[t] + decay_rate × adstock[t-1]

Saturation Curves

Applies logarithmic transformation to model diminishing returns: log(1 + adstock)
Captures the non-linear relationship between spend and impact

Temporal Features

Cyclical encoding using sine/cosine transformations for seasonality
Week-of-year and month indicators for periodic patterns
Time index for long-term trends

Interaction Effects

Creates promotional interaction features (promotions × media spend)
Lag features for media channels, emails, and SMS (1-3 week lags)

2. Two-Stage Modeling (Causal Inference)
Stage 1: Mediator Model

Predicts Google spend based on other media channels and temporal features
Addresses endogeneity in media planning decisions
Generates predicted values and residuals for causal analysis

Stage 2: Revenue Model

Uses both predicted Google spend and residuals from Stage 1
Incorporates all media channels, control variables, and engineered features
Separates causal effects from correlational relationships

3. Hyperparameter Optimization
Optuna-based Tuning

Optimizes XGBoost parameters for each decay rate tested
50 trials per configuration with early stopping
Parameters tuned: learning rate, max depth, regularization (lambda, alpha), sampling rates

Model Selection

Evaluates each decay rate + hyperparameter combination
Selects best model based on R² score on test set
Uses time series split to prevent data leakage

4. Model Evaluation
Performance Metrics

R-squared for explained variance
MAE, RMSE for prediction accuracy
MAPE for percentage-based error assessment

Validation Strategy

80/20 train-test split respecting temporal order
Early stopping to prevent overfitting
Cross-validation with time series splits

Key Technical Features

XGBoost Regressor with advanced regularization
Geometric adstock for media carryover effects
Two-stage causal modeling to handle endogeneity
Automated hyperparameter tuning with Optuna
Time series validation to prevent leakage
Interactive visualization with Plotly

Results
The model optimizes across different adstock decay rates and provides:

Best performing decay rate selection
Feature importance rankings
Revenue predictions with uncertainty bounds
Visual comparison of actual vs predicted performance
