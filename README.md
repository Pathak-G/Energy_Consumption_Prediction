README – Instructions for Running the Code

# Learning From Limited Data in Energy Modelling

**CS6472 – Research Methods and Specifications | Assignment 4 | PS15**  
Govind Pathak · Tarun Pathuri · Yanan Zia · Zeqi Yu · University of Limerick

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Requirements](#2-requirements)
3. [File Structure](#3-file-structure)
4. [Setup & Installation](#4-setup--installation)
5. [How to Run](#5-how-to-run)
6. [Pipeline Walkthrough](#6-pipeline-walkthrough)
7. [Data Sources](#7-data-sources)
8. [Known Issues](#8-known-issues)
---

## 1. Project Overview
   
This project predicts daily household energy consumption (`Energy_Consumption_kWh`) using machine learning.  
The research question investigates **how augmenting a large public dataset with a small student-collected dataset affects prediction performance**, and at what data volume synthetic augmentation provides a measurable benefit.

**Target variable:** `Energy_Consumption_kWh`  

**Evaluation metric:** Root Mean Squared Error (RMSE)  

**Best result:** RMSE = 0.517 on the public Kaggle leaderboard

---
## 2. Requirements
### Python Version
```
Python >= 3.9
```

Required libraries include:

```
pandas >= 1.5
numpy >= 1.23
scikit-learn >= 1.1
xgboost >= 1.7
requests
matplotlib >= 3.5
seaborn >= 0.12
```
> **Note:** An **internet connection** is required. The notebook calls the Open-Meteo Archive API to fetch historical temperature data for the synthetic and test datasets.


---
## 3. File Structure

Place all files in the **same directory** before running:

```
project/
├── EnergyPrediction_Final.ipynb        ← Main notebook (run this)
├── household_energy_consumption.csv    ← Kaggle training dataset (90,000 rows)
├── synthetic_release.csv               ← Student-collected dataset (390 rows)
├── test_data.csv                       ← Unlabelled test set (390 rows)
├── submission.csv                      ← Generated after running the notebook
├── EnergyPredictionPoster.pdf          ← Project summary poster
└── README.md                           ← This file
```

---

## 4. Setup & Installation

### Option A — Google Colab (Recommended)

All required libraries are pre-installed in Colab. No extra installation needed.

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Upload `EnergyPrediction_Final.ipynb`
3. Upload the three CSV files to the Colab session storage:
   - Click the **folder icon** in the left sidebar
   - Click **Upload** and select all three CSV files
4. Set the runtime to GPU:  
   `Runtime → Change runtime type → T4 GPU → Save`

### Option B — Local (Jupyter)

Install dependencies:

```bash
pip install pandas numpy scikit-learn xgboost requests matplotlib seaborn
```

Then launch the notebook:

```bash
jupyter notebook EnergyPrediction_Final.ipynb
```

---

## 5. How to Run

### Step 1 — Open the notebook

Open `EnergyPrediction_Final.ipynb` in Colab or Jupyter.

### Step 2 — Run all cells

**In Colab:**
```
Runtime → Run all
```

**In Jupyter:**
```
Kernel → Restart & Run All
```

### Step 3 — Wait for API calls to complete

Section 7 of the notebook fetches temperature data from the Open-Meteo API.  
This makes one request per unique `(County, Date)` pair — expect this step to take **1–3 minutes** depending on your connection speed.

### Step 4 — Collect the submission file

Once the final cell runs, a file named `submission.csv` will be saved in the same directory.  
This is the Kaggle competition submission file.

**In Colab**, download it:
- Click the **folder icon** in the left sidebar
- Right-click `submission.csv` → **Download**

### Step 5 — Submit to Kaggle

Upload `submission.csv` to the Kaggle competition page.

---

## 6. Pipeline Walkthrough

The notebook runs the following steps in order:

| Section | What it does |
|---------|-------------|
| 1 | Imports all libraries and sets the colorblind-safe colour palette globally |
| 2 | Loads `household_energy_consumption.csv`, `synthetic_release.csv`, `test_data.csv` |
| 3 | Plots the distribution of `Energy_Consumption_kWh` (EDA) |
| 4 | Cleans the messy `County` column (fixes typos, removes non-county entries, imputes with mode) |
| 5 | Parses dates and extracts `Month` and `DayOfWeek` features |
| 6 | Imputes missing features: `Household_Size = 1`, `Peak_Hours_Usage_kWh = 4.3196` (Kaggle mean) |
| 7 | Calls the Open-Meteo API to fetch real daily temperatures per `(County, Date)` pair |
| 8 | Label-encodes `Has_AC` (Yes → 1, No → 0) |
| 9 | Merges Kaggle + Synthetic into one training set; defines 6 features and applies 80/20 split |
| 10 | Trains four baseline models: Linear Regression, Random Forest, Gradient Boosting, XGBoost |
| 11 | Evaluates all models with RMSE and plots results |
| 12 | Ablation study — trains XGBoost at 25/50/75/100% of data for both Kaggle-only and combined sets |
| 13 | Re-trains the final XGBoost on 100% of data and saves `submission.csv` |

---

## 7. Data Sources


• Kaggle Dataset : 90,000 rows, 7 features

• Synthetic Dataset : 390 rows, student‑collected

• Test Dataset : 390 rows, unlabeled (competition format)

---

## 8. Known Issues

**API timeout**  
If the Open-Meteo API times out, the affected rows will have `NaN` for `Avg_Temperature_C`. Re-running the cell usually resolves this.

**Date format warning**  
The test dataset uses `DD/MM/YYYY` format (with some erroneous years like 1990). These are parsed correctly by the notebook using `dayfirst=True`, but Pandas may print a warning — this can be safely ignored.

**Colab session reset**  
If your Colab session disconnects before the notebook finishes, re-upload the CSV files and run all cells again from the top.

