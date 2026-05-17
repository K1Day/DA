# 📊 Student Performance Statistical Analysis

Statistical hypothesis testing project analyzing exam scores of 1,000 students across math, reading, and writing subjects.

---

## Files

| File | Description |
|------|-------------|
| `Students_scores_analysis.ipynb` | Main analysis notebook |
| `StudentsPerformance_1.csv` | Dataset (1,000 students) |

---

## Dataset

| Column | Description |
|--------|-------------|
| `gender` | Student gender (male / female) |
| `race/ethnicity` | Student group (A–E) |
| `parental level of education` | Highest education level of parent |
| `lunch` | Lunch type (standard / free/reduced) |
| `test preparation course` | Whether the student completed a prep course |
| `math score` | Math exam score (0–100) |
| `reading score` | Reading exam score (0–100) |
| `writing score` | Writing exam score (0–100) |

> No null values or duplicates found in the dataset.

---

##  Research Questions & Hypothesis Tests

### 1. Is the average math score different from 65?

- **H₀:** μ = 65
- **H₁:** μ ≠ 65
- **Test:** One-sample t-test
- **Result:** Mean = 66.09, t = 2.27, **p = 0.02**  ✅ Reject H₀

Students score statistically significantly above the 65-point threshold.
<img width="1489" height="490" alt="image" src="https://github.com/user-attachments/assets/69095a6d-2f0f-46d0-b374-12324f42abdf" />

---

### 2. Does test preparation improve math scores?

- **H₀:** μ_prepared ≤ μ_unprepared
- **H₁:** μ_prepared > μ_unprepared
- **Test:** One-tailed Welch's t-test
- **Result:** Prepared = 69.7, Unprepared = 64.1, **p ≈ 0.0000**  ✅ Reject H₀

The prep course provides a meaningful ~5–6 point boost.
<img width="1489" height="490" alt="image" src="https://github.com/user-attachments/assets/efa41690-7e0a-4a18-95a7-f6826f1014bd" />

---

### 3. Do students read better than they write?

- **H₀:** μ_reading = μ_writing
- **H₁:** μ_reading ≠ μ_writing
- **Test:** Paired t-test
- **Result:** Reading = 69.17, Writing = 68.05, diff = 1.11, **p ≈ 0.0000** ✅ Reject H₀

Students read slightly but significantly better than they write.
<img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/ca76d9c6-6384-45a1-be2e-d5cdba2afba2" />


---

### 4. Is there a gender gap in math excellence (score > 80)?

- **H₀:** p_male = p_female
- **H₁:** p_male ≠ p_female
- **Test:** Two-proportion z-test
- **Result:** Males = 22%, Females = 13%, z = 3.85, **p = 0.0001** ✅ Reject H₀

Males are significantly more likely to score above 80 in math.
<img width="1489" height="490" alt="image" src="https://github.com/user-attachments/assets/52727c5e-927a-4df5-8924-c5b091683628" />

---

## 📌 Key Findings

- ✅ Preparatory courses are effective — average boost of **+5–6 points**
- ✅ Students generally perform **above the 65-point** math threshold
- ✅ Reading and writing skills are nearly equal — slight **reading advantage**
- ✅ Male students are significantly more likely to be **high achievers in math**
- All results are statistically significant (large sample, n = 1,000)

---

## Libraries Used

```
pandas · numpy · scipy · matplotlib · seaborn · statsmodels
```

## How to Run

```bash
pip install pandas numpy scipy matplotlib seaborn statsmodels
jupyter notebook Students_scores_analysis.ipynb
```
---

<br>

---
# California Housing Price Analysis

## Overview

This project analyzes the **California Housing Dataset** to build a linear regression model predicting median house prices. The notebook covers the full ML pipeline: exploratory data analysis, feature engineering, multicollinearity handling, model training, and evaluation.

---

## Dataset

The dataset contains **20,640 records** with the following features:

| Feature | Description | Type |
|---|---|---|
| `MedInc` | Median income of the block (in $10,000s) | float64 |
| `HouseAge` | Median age of houses in the block | float64 |
| `AveRooms` | Average number of rooms per household | float64 |
| `AveBedrms` | Average number of bedrooms per household | float64 |
| `Population` | Block population | float64 |
| `AveOccup` | Average number of occupants per household | float64 |
| `Latitude` | Block latitude | float64 |
| `Longitude` | Block longitude | float64 |
| `MedHouseVal` | **Target**: Median house value (in $100,000s) | float64 |

No missing values were found in the dataset.

---

## Exploratory Data Analysis

**Key correlations discovered:**

- **MedInc ↔ MedHouseVal (r = 0.69)** — The strongest predictor: higher neighborhood income means more expensive houses.
- **AveRooms ↔ AveBedrms (r = 0.85)** — Strong collinearity; larger houses naturally have more bedrooms.
- **Latitude ↔ Longitude (r = −0.92)** — Near-perfect inverse relationship reflecting California's geography.
- **HouseAge ↔ Population (r = −0.30)** — Older neighborhoods tend to be less populated.
<img width="490" height="510" alt="image" src="https://github.com/user-attachments/assets/bdfcbe09-fcf8-4888-b603-941f0112911e" />

**Geographic patterns:**
Price peaks appear around latitudes 34° and 37–38° (Los Angeles and San Francisco areas). The most expensive properties cluster near longitude −118° (the coast), with prices declining inland.

**Outliers:**
Properties with 20–140 average rooms are not mansions — they are dormitories or hotels included in the housing statistics, which skew the AveRooms distribution.

---

## Feature Engineering

To address multicollinearity:

- **Removed** `AveRooms` and `AveBedrms` (VIFs of 19.12 and 14.36 respectively).
- **Created** `bedroom_ratio` = AveBedrms / AveRooms — captures bedroom density without the collinearity.
- After the replacement, VIF for `MedInc` dropped from 2.67 → 2.04, and `bedroom_ratio` achieved a VIF of 1.87.

The feature `AveBedrms` was also found statistically insignificant (p-value = 0.948) and was excluded.
<img width="490" height="510" alt="image" src="https://github.com/user-attachments/assets/2d4536d0-b948-4eec-b8b4-6485635a63d5" />

---

## Modeling

### Baseline Linear Regression

| Split | R² |
|---|---|
| Train | 0.607 |
| Test | 0.601 |

Minimal gap between train and test indicates no overfitting.

### OLS with Feature Engineering

After adding `bedroom_ratio` and removing collinear features:

| Split | R² |
|---|---|
| Train | 0.548 |
| Test | 0.528 |

A square root transformation was applied to the target variable (`MedHouseVal`) to reduce the effect of outliers. The average RMSE, converted back to real-world values, is approximately **$130,000**.

---

## Results & Limitations

**What worked:**
- Clean feature set with reduced multicollinearity.
- Stable model with no overfitting.
- `MedInc`, `Latitude`, `Longitude`, and `bedroom_ratio` are the most significant predictors (all p < 0.001).

**Limitations:**
- **Heteroscedasticity** — The residual plot shows the model performs well on cheaper houses but significantly underestimates prices for expensive properties. Errors grow with predicted value.
- R² of ~0.53–0.60 means roughly 40–47% of price variation remains unexplained by the linear model.
<img width="576" height="463" alt="image" src="https://github.com/user-attachments/assets/1cc88cc2-adb0-42b7-9409-e913c0b356e5" />
<img width="505" height="510" alt="image" src="https://github.com/user-attachments/assets/474701f9-825e-4e94-995b-a7e8ebd41438" />
<img width="572" height="453" alt="image" src="https://github.com/user-attachments/assets/95a4cf98-9d0d-41b3-ac5b-5ff43adc6ae7" />

**Possible improvements:**
- Try log transformation of the target variable instead of square root.
- Remove outliers in the high-price segment.
- Use non-linear models (Random Forest, Gradient Boosting) to capture complex interactions.

---

## Project Structure

```
House_analysis.ipynb   # Main analysis notebook
README.md              # Project documentation
```

## Requirements

```
pandas
numpy
scikit-learn
statsmodels
matplotlib
seaborn
```
