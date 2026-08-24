# ED Congestion Prediction

> **Predicting Emergency Department Congestion with Synthetic Time Series and Machine Learning**

[![KSC 2025](https://img.shields.io/badge/KSC-2025-blue)](포스터-PDF-링크)

공공 응급의료 통계 데이터를 **3시간 간격의 Synthetic Time Series**로 재구성하고,
XGBoost · Random Forest · CNN-LSTM을 비교하여 **응급실 혼잡도를 예측**한 프로젝트입니다.

**XGBoost가 가장 낮은 MAE와 RMSE를 기록하며 가장 우수한 성능을 보였습니다.**

본 연구는 **한국정보과학회 KSC 2025**에서 포스터로 발표되었습니다.

---

## 📌 Project Overview

공개된 응급의료 데이터는 연도별·시간대별 **요약 통계 형태**로 제공되어
일반적인 시계열 예측 모델에 바로 적용하기 어렵다는 한계가 있습니다.

이를 해결하기 위해 다음과 같은 예측 파이프라인을 구성했습니다.

**응급의료 요약 통계**
→ **Synthetic Time Series 생성**
→ **기상·공휴일 데이터 결합**
→ **시계열 Feature Engineering**
→ **모델 학습 및 비교**
→ **향후 3시간 환자 수 예측**

---

## 📊 Dataset

| Data          | Description       |
| ------------- | ----------------- |
| 서울시 응급의료 통계   | 2020.01 ~ 2024.12 |
| 기상청 ASOS      | 기온, 강수량 등         |
| Calendar Data | 공휴일, 요일 및 시간 정보   |

연간 단위의 응급의료 요약 통계를
계절성·변동성·연속성을 고려하여 **3시간 간격의 시계열 데이터**로 재구성했습니다.

---

## ⚙️ Methodology

### 1. Synthetic Time Series

시계열 분석이 어려운 연간 요약 통계를
**3시간 간격의 연속적인 Synthetic Time Series**로 재구성했습니다.

### 2. External Data Integration

응급실 혼잡도에 영향을 줄 수 있는 외부 데이터를 결합했습니다.

* 기상청 ASOS 관측 데이터
* 공휴일 여부
* 요일 및 시간 정보

기상 데이터는 **3시간 간격으로 리샘플링**하고 결측치를 처리했으며,
공휴일 정보는 이진 변수로 변환했습니다.

### 3. Feature Engineering

과거 혼잡도의 영향과 단기 추세를 반영하기 위해
시계열 기반 Feature를 추가했습니다.

| Feature        | Description                |
| -------------- | -------------------------- |
| Lag Features   | 3h, 6h, 18h, 최대 72h 이전 혼잡도 |
| Moving Average | 최근 18시간 평균                 |
| Moving Std     | 최근 18시간 표준편차               |

### 4. Prediction Structure

과거 **36시간(12 timesteps)**의 데이터를 입력으로 사용해
**향후 3시간의 환자 수**를 예측했습니다.

```text
Past 36 hours
[x(t-12), ..., x(t-1)]
        ↓
      Model
        ↓
Patient Volume at t
```

### 5. Train / Test Split

데이터 누수를 방지하기 위해 데이터를 시간 순서대로 분할했습니다.

* **Train:** 80%
* **Test:** 20%
* **Scaling:** MinMaxScaler

---

## 🤖 Models

총 3개의 모델을 비교했습니다.

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

### Best Model — XGBoost

**XGBoost가 MAE와 RMSE 모두 가장 낮은 값을 기록했습니다.**

제한된 데이터 환경에서는 복잡한 딥러닝 모델보다
**도메인 기반 Feature Engineering + XGBoost** 조합이
더 효과적인 예측 방법이 될 수 있음을 확인했습니다.

---

## 💡 Key Contributions

* 요약 통계를 시계열 분석에 활용하기 위한
  **Synthetic Time Series 재구성**

* 기상·공휴일·과거 혼잡도 정보를 활용한
  **도메인 기반 Feature Engineering**

* **XGBoost · Random Forest · CNN-LSTM** 성능 비교

* 공개 통계 데이터만으로도 활용 가능한
  **응급실 혼잡도 예측 방법의 가능성 분석**

---

## 🛠 Tech Stack

`Python` `Pandas` `Scikit-learn` `XGBoost`

---

## 👥 Team

**성신여자대학교 AI융합학부 4인 공동 연구**

* 이예림
* 김한솜
* 나유진
* 오윤지


데이터 전처리, Feature Engineering, 모델 학습 및 성능 평가를
팀원 전원이 공동으로 수행했습니다.

---

## 🏆 Publication

**KSC 2025 — 한국정보과학회 한국소프트웨어종합학술대회**

본 연구는 **KSC 2025 포스터 세션**에서 발표되었습니다.


