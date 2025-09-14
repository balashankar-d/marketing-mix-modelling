# 📊 Media Mix Modeling with XGBoost

> A marketing analytics solution that optimizes media spend allocation across multiple channels using advanced feature engineering, causal inference, and machine learning.

## 🚀 Overview

This project implements **Media Mix Modeling (MMM)** using XGBoost with causal inference techniques to accurately measure the incremental impact of different media channels on revenue.

The pipeline leverages geometric adstock transformations, saturation curves, and two-stage causal modeling to address endogeneity and provide actionable insights for optimal budget allocation.

---

## 🏗️ Approach

### 🔧 1. Feature Engineering

#### 📈 Adstock Modeling
Captures carryover effects of advertising using geometric adstock transformation:

```
adstock[t] = spend[t] + decay_rate × adstock[t-1]
```

- Tests multiple decay rates: `0.1, 0.3, 0.5, 0.7, 0.9`
- Selects optimal decay rate based on model performance

#### 📉 Saturation Curves
Models diminishing returns on spend:

```
saturation = log(1 + adstock)
```

This captures the non-linear relationship between spend and its incremental impact.

#### ⏳ Temporal Features
- Cyclical encoding of seasonality using sine/cosine transformations
- Week-of-year and month indicators for periodic effects
- Time index to capture long-term trends

#### 🔄 Interaction Effects
- **Promotional interactions**: `(promotion × media_spend)`
- **Lag features**: 1–3 week lags for media, email, and SMS campaigns

### 🧠 2. Two-Stage Causal Modeling

#### Stage 1: Mediator Model
- Predicts Google spend using other media channels + temporal features
- Addresses endogeneity in spend allocation decisions
- Generates predicted values & residuals for causal inference

#### Stage 2: Revenue Model
- Uses predicted Google spend + residuals from Stage 1
- Incorporates:
  - All media channels
  - Control variables
  - Engineered features
- Separates causal effects from correlational effects

### 🎯 3. Hyperparameter Optimization

#### Optuna-based Tuning
- 50 trials per decay rate configuration with early stopping
- **Parameters tuned:**
  - `learning_rate`
  - `max_depth`
  - `lambda`, `alpha` (regularization)
  - Sampling rates

---

## 📊 Model Selection & Validation

### Model Selection
- Choose best decay rate + hyperparameter combination
- Based on R² score on test set

### Validation Strategy
- 80/20 time-ordered train-test split
- Cross-validation with time-series splits
- Early stopping to prevent overfitting

---

## 📈 Model Evaluation

| Metric | Purpose |
|--------|---------|
| **R²** | Explained variance |
| **MAE** | Mean Absolute Error |
| **RMSE** | Root Mean Squared Error |
| **MAPE** | Percentage-based error assessment |

---

## 🛠️ Key Technical Features

- ✅ **XGBoost Regressor** with regularization
- ✅ **Geometric Adstock** for carryover effects
- ✅ **Two-Stage Causal Modeling** to handle endogeneity
- ✅ **Optuna Hyperparameter Tuning**
- ✅ **Time Series Validation** to avoid leakage
- ✅ **Interactive Visualizations** with Plotly

---

## 📊 Results

The model provides:

- ✅ **Optimal Decay Rate** automatically selected
- ✅ **Feature Importance Rankings** for media channels
- ✅ **Revenue Predictions** with uncertainty bounds
- ✅ **Visual Comparison** of actual vs. predicted performance

---
