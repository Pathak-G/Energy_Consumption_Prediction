Energy Consumption Prediction - Learning From Limited Data


Overview

This project explores machine learning approaches for predicting household energy consumption under limited data conditions. The study evaluates real (Kaggle) and synthetic datasets, investigates data scarcity challenges, and tests whether synthetic augmentation improves model generalisation.

---

Datasets

• Kaggle Dataset : 90,000 rows, 7 features

• Synthetic Dataset : 390 rows, student‑collected

• Test Dataset : 390 rows, unlabeled (competition format)

---
Feature Engineering

• Cleaned and standardised County names

• Extracted Month and DayOfWeek

• Imputed missing features: Household_Size = 1, Peak_Hours_Usage_kWh = 4.3196

• Optimised API temperature retrieval using unique (County, Date) pairs

• Encoded Has_AC (Yes/No -> 1/0)

• Final feature set: 6 engineered features

---
Models & Experiments

• Baseline models: Linear Regression, Random Forest, Gradient Boosting, XGBoost

• XGBoost performed best (RMSE = 0.7556)

• Kaggle‑only training achieved RMSE = 0.6386

• Synthetic data helped only at very small fractions

• Final tuned XGBoost (3000 trees, LR=0.001) achieved RMSE = 0.517

---
Conclusion

High‑quality feature engineering and careful model tuning enabled strong performance even with limited data. Synthetic data is useful only in low‑data regimes, and temperature retrieval via API significantly improved model accuracy.
---

Contributors

• Govind Pathak

• Tarun Pathuri

• Yanan Zia

• Zeqi Yu
