# Heart Disease Determinants — Logistic Regression Analysis (R)

A statistical analysis identifying the clinical and demographic factors most strongly associated with heart disease risk, using logistic regression on a 270-patient medical dataset (14 variables).

## Motivation

Cardiovascular disease is one of the leading causes of death worldwide. This project analyzes which clinical indicators and demographic factors most increase the probability of diagnosis, moving from a simple age-only model to a full multivariate specification incorporating qualitative clinical markers (chest pain type, vessel status, ECG findings).

## Methodology

1. **Data preparation** — recoded the target variable to binary, converted categorical clinical variables (sex, chest pain type, fasting blood sugar, exercise-induced angina) to factors.
2. **EDA** — correlation matrix of quantitative predictors; age-density distribution by disease status.
3. **Linear Probability Model (LPM)** — baseline OLS on the binary target; Breusch-Pagan test confirmed heteroskedasticity, motivating the switch to logit.
4. **Nested logit models (A → D)** — progressively added demographic → cardiovascular → clinical/ECG variables; compared via likelihood-ratio tests and AIC.
5. **Interaction model** — tested whether age's effect on disease risk differs by sex.
6. **Diagnostics** — VIF (multicollinearity), RESET test and link test (functional form), Cook's distance (influential observations).
7. **Classification quality** — confusion matrix, ROC/AUC, Hosmer-Lemeshow calibration test, decile-based calibration check.

## Key Results

**Final model (Model D):** `target ~ age + sex + chest pain type + major vessels + oldpeak + max heart rate`

- Lowest AIC (222.2) among all tested specifications; each added variable block statistically significant (p < 0.001, LR test).
- **AUC = 0.908** — excellent discriminative ability.
- **Classification accuracy = 82.2%** (222/270 correctly classified).
- Hosmer-Lemeshow test (p = 0.820) confirms good calibration.
- VIF range 1.04–1.15 → no meaningful multicollinearity.

### Odds Ratios

| Predictor | Odds Ratio | Interpretation |
|---|---|---|
| Sex (male) | 5.26 | Men have 5.26× the odds of disease vs. women |
| Chest pain type 4 | 11.10 | Strongest single predictor in the model |
| Major vessels (per vessel) | 2.66 | Each additional blocked vessel |
| Oldpeak (per unit) | 2.08 | ECG ST-depression measure |
| Max heart rate (per bpm) | 0.98 | Mild protective effect |

**Notable finding:** age is significant in isolation, but its effect is fully mediated once clinical variables (vessel status, ECG) enter the model — age itself is not the direct cause, but correlates with the physiological deterioration that clinical markers capture directly. The age × sex interaction was not significant, indicating the age effect is consistent across sexes.

## Tools

R — `dplyr`, `ggplot2`, `lmtest`, `car`, `stargazer`, `pROC`, `margins`, `corrplot`, `ResourceSelection`

## Files

- `analysis.Rmd` — full R Markdown analysis (knits to HTML with table of contents and code folding)
- `data/dataset_heart.xlsx` — dataset used in the analysis (270 patients, 14 variables)

## Data Source

270-patient heart disease dataset with 14 clinical/demographic variables (age, sex, chest pain type, resting blood pressure, serum cholesterol, fasting blood sugar, resting ECG, max heart rate, exercise-induced angina, oldpeak, ST segment, major vessels, thalassemia, heart disease status).
