
[한국어 README ](./README_Korean.md)


# ED Congestion Prediction

> **Predicting Emergency Department Congestion with Synthetic Time Series and Machine Learning**

[![KSC 2025](https://img.shields.io/badge/KSC-2025-blue)](POSTER-PDF-LINK)

A machine learning project that reconstructs public emergency medical statistics into a **3-hour-interval synthetic time series** and predicts emergency department congestion by comparing multiple machine learning and deep learning models.

Among **XGBoost, Random Forest, and CNN-LSTM**, **XGBoost achieved the lowest MAE and RMSE**, showing the best predictive performance.

This research was presented as a poster at **KSC 2025, the Korea Software Congress**.

---

## 📌 Project Overview

Publicly available emergency medical datasets are often provided as **aggregated statistics by year or time period**, making them difficult to directly apply to conventional time-series forecasting models.

To address this limitation, we designed the following prediction pipeline:

**Aggregated Emergency Medical Statistics**
→ **Synthetic Time Series Generation**
→ **Weather & Holiday Data Integration**
→ **Time-Series Feature Engineering**
→ **Model Training & Comparison**
→ **3-Hour-Ahead Patient Volume Prediction**

---

## 📊 Dataset

| Data                               | Description                                 |
| ---------------------------------- | ------------------------------------------- |
| Seoul Emergency Medical Statistics | Jan. 2020 – Dec. 2024                       |
| KMA ASOS Weather Data              | Temperature, precipitation, etc.            |
| Calendar Data                      | Holidays, day of week, and time information |

The annual aggregated emergency medical statistics were reconstructed into a
**3-hour-interval synthetic time series** while considering seasonality, variability, and continuity.

---

## ⚙️ Methodology

### 1. Synthetic Time Series Generation

Aggregated annual statistics, which were not directly suitable for time-series analysis,
were reconstructed into a continuous **3-hour-interval synthetic time series**.

### 2. External Data Integration

External variables potentially related to emergency department congestion were integrated into the dataset.

* KMA ASOS weather observations
* Holiday indicators
* Day-of-week and time features

Weather observations were resampled at **3-hour intervals** and missing values were handled.
Holiday information was converted into binary features.

### 3. Feature Engineering

Time-series features were generated to capture both residual effects of previous congestion and short-term trends.

| Feature        | Description                                               |
| -------------- | --------------------------------------------------------- |
| Lag Features   | Congestion levels from 3h, 6h, 18h, and up to 72h earlier |
| Moving Average | Rolling average over the previous 18 hours                |
| Moving Std     | Rolling standard deviation over the previous 18 hours     |

### 4. Prediction Structure

The model uses the previous **36 hours (12 timesteps)** of feature data
to predict patient volume for the **next 3-hour interval**.

```text
Past 36 hours
[x(t-12), ..., x(t-1)]
        ↓
      Model
        ↓
Patient Volume at t
```

### 5. Train / Test Split

To prevent data leakage, the dataset was split chronologically.

* **Train:** 80%
* **Test:** 20%
* **Scaling:** MinMaxScaler

---

## 🤖 Models

Three predictive models were evaluated:

* **XGBoost**
* Random Forest
* CNN-LSTM

### XGBoost Hyperparameters

```text
n_estimators = 400
max_depth = 6
learning_rate = 0.05
subsample = 0.8
colsample_bytree = 0.8
```

---

## 📈 Results

| Model         |      MAE ↓ |     RMSE ↓ |
| ------------- | ---------: | ---------: |
| **XGBoost**   | **59.381** | **68.298** |
| Random Forest |     75.317 |     87.198 |
| CNN-LSTM      |    128.263 |    147.019 |

### Best Performing Model — XGBoost

**XGBoost achieved the lowest MAE and RMSE among the three models.**

The results suggest that, in a limited-data setting, the combination of
**domain-informed feature engineering and XGBoost** can provide a more effective and resource-efficient alternative to a more complex deep learning model.

---

## 💡 Key Contributions

* Reconstructed aggregated emergency medical statistics into a
  **Synthetic Time Series** suitable for time-series forecasting

* Integrated weather, holiday, and historical congestion information through
  **domain-informed feature engineering**

* Compared the predictive performance of
  **XGBoost, Random Forest, and CNN-LSTM**

* Demonstrated the feasibility of building an
  **emergency department congestion prediction system using publicly available statistical data**

---

## 🛠 Tech Stack

`Python` `Pandas` `Scikit-learn` `XGBoost`

---

## 👥 Team

**Department of AI Convergence, Sungshin Women's University**

4-person collaborative research project:

* Hansom Kim
* Yujin Na
* Yunji Oh
* Yerim Lee

Data preprocessing, feature engineering, model training, and performance evaluation
were conducted collaboratively by all team members.

---

## 🏆 Publication

**KSC 2025 — Korea Software Congress**

This research was presented in the **poster session at KSC 2025**.

📄 [KSC 2025 Poster](POSTER-PDF-LINK)
