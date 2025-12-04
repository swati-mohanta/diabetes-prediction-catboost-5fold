# 🩺 Diabetes Prediction Model (Kaggle Competition)

This repository contains my complete workflow for the **Kaggle Diabetes Prediction Challenge**.
The goal of the competition is to build a machine learning model that predicts whether a person has **diagnosed diabetes** based on demographic, lifestyle, and medical measurements.

This project uses:

* **Feature Engineering**
* **Stratified 5-Fold Cross-Validation**
* **CatBoost Classifier** (handles categorical features extremely well)
* **OOF (Out-Of-Fold) Evaluation**

---

## 📂 Dataset Overview

The dataset includes **700,000 rows** with features such as:

### **Demographic**

* age
* gender
* ethnicity
* education_level
* income_level
* employment_status

### **Lifestyle**

* physical_activity_minutes_per_week
* alcohol_consumption_per_week
* sleep_hours_per_day
* screen_time_hours_per_day
* smoking_status

### **Medical**

* bmi
* waist_to_hip_ratio
* systolic_bp / diastolic_bp
* cholesterol levels (HDL, LDL, Total, Triglycerides)
* family_history_diabetes
* hypertension_history
* cardiovascular_history

### **Target Column**

* `diagnosed_diabetes` (0 or 1)

---

## 🛠️ Feature Engineering

A major portion of predictive performance came from creating meaningful features.
Engineering steps include:

### **📌 Numeric Transformations**

* BMI squared, BMI log transform
* Age × BMI
* Activity × BMI
* Cholesterol ratios:

  * LDL/HDL
  * TG/HDL
  * TotalChol/HDL
* SystolicBP / Age
* Activity per BMI
* Sleep per Activity

### **📌 Categorical Binning**

* Age groups (≤30, 31-45, 46-60, 60+)
* BMI categories (underweight, normal, overweight, obese)
* Activity level buckets
* Alcohol intake categories
* Sleep category (short, optimal, long)
* Screen-time buckets
* Waist-to-hip ratio buckets
* Blood pressure stage buckets

### **📌 Flags**

* has_family_history
* has_hypertension
* is_smoker

These features allow CatBoost to learn non-linear interactions more effectively.

---

## 🔁 Model Training — Stratified 5-Fold Cross-Validation

I trained a **CatBoostClassifier** using:

* 1800 iterations
* depth = 8
* learning rate = 0.02
* border_count = 254
* bagging_temperature = 0.2
* eval metric = accuracy
* categorical features passed by index

Each fold trains on 80% and validates on 20%, preserving class balance via **stratification**.

### ✔ Out-of-Fold (OOF) predictions were generated

### ✔ Test predictions were averaged across all 5 folds

---

## 🧾 Final Results

Using feature engineering + 5-fold CatBoost:

```
Mean CV Accuracy: 0.681679  
Std: 0.000885  
OOF Accuracy (from combined OOF preds): 0.681679
```

This indicates:

* The model is **stable across folds** (very low standard deviation)
* The dataset has **weak but consistent signal**
* Feature engineering significantly improves robustness

---

## 📊 Submission

The notebook saves a `submission.csv` in Kaggle-compatible format:

```
id, diagnosed_diabetes
700000, 1
700001, 1
700002, 1
...
```

You can upload this file directly on the Kaggle competition page.

---

## 🚀 How to Run This Project

### **1. Clone the repository**

```bash
git clone https://github.com/<swati-mohanta>/<diabetes-prediction-catboost-5fold>.git
cd <repo-name>
```

### **2. Install Dependencies**

```bash
pip install -r requirements.txt
```

Required libraries:

* pandas
* numpy
* catboost
* scikit-learn
* matplotlib / seaborn (optional for EDA)

### **3. Run the Notebook**

Open the Jupyter notebook:

```bash
jupyter notebook
```

or run directly on Kaggle Notebooks.

---

## 🏆 Acknowledgements

This project was built as part of the **Kaggle Diabetes Prediction Competition**, and is publicly available for educational and research use.

---
