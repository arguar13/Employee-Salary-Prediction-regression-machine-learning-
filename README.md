*Read this in other languages: [Español](README_es.md)*
# Machine Learning Project - Employee Salary Prediction

This project develops an advanced, end-to-end Machine Learning regression pipeline designed to predict employee salaries using professional, educational, and job-related information.

The objective is to build a scalable and production-ready salary estimation system through robust data preprocessing, custom feature engineering, dynamic model benchmarking, and automated hyperparameter optimization.

---

# About the Dataset

| Attribute | Description |
|------------|------------|
| **Name** | Employee Salary Prediction Dataset |
| **Target Variable** | `salary` |
| **File Type** | CSV |
| **Volume** | 250,000 records |
| **Attributes** | 9 features + 1 target |

The dataset contains comprehensive employee profile and job information. The features utilized in this analysis include:

| Feature | Description |
|----------|-------------|
| `job_title` | The professional designation of the employee. |
| `experience_years` | Years of professional experience. |
| `education_level` | Highest educational degree attained (High School, Diploma, Bachelor, Master, PhD). |
| `skills_count` | Total number of documented technical or professional skills. |
| `industry` | The sector in which the company operates. |
| `company_size` | Organization size (Startup, Small, Medium, Large, Enterprise). |
| `location` | Geographical location of the role. |
| `remote_work` | Modality of work (Yes, No, Hybrid). |
| `certifications` | Number of professional certifications held. |

---

# Project Workflow

The project follows a rigorous, professional data science workflow:

1. **Data Acquisition and Cleaning**
   - Initial data ingestion, validation, and duplication removal.

2. **Exploratory Data Analysis (EDA)**
   - Statistical analysis of the target variable and categorical relationships.

3. **Data Splitting**
   - Strict Train/Validation/Test split (64% / 16% / 20%).

4. **Custom Transformers and Preprocessing**
   - Modular pipeline construction utilizing custom scikit-learn classes.

5. **Dynamic Model Benchmarking**
   - Comparing multiple algorithms on the hold-out validation set to establish the strongest baseline.

6. **Hyperparameter Tuning**
   - Automated search space exploration using Optuna.

7. **Final Evaluation**
   - Assessing the best model on unseen test data to guarantee generalizability.

8. **Error Analysis & Interpretability**
   - Breaking errors down by segment and measuring which variables drive the predictions (permutation importance).

---

# Preprocessing and Feature Engineering

The pipeline is fully modularized utilizing `scikit-learn Pipelines` and `ColumnTransformer`.

Custom `BaseEstimator` and `TransformerMixin` classes were developed to handle specific domain requirements.

## Feature Engineering

- Custom `FeatureEngineer` adding ordinal ranks for `education_level` and `company_size`, so the natural ordering of these categories is available to the models alongside their one-hot encoding.

## Numerical Pipeline

### Missing Value Handling
- Median imputation.

### Outlier Treatment
- Custom `IQRTransformer` applying clipping based on the Interquartile Range (IQR) method with a 1.5 factor.

### Scaling
- Standardization using `StandardScaler`.

## Categorical Pipeline

### Missing Value Handling
- Most-frequent imputation.

### Dimensionality Reduction
- Custom `RareCategoryEncoder` grouping infrequent categorical modalities (threshold < 5%) into a unified `"Other"` category.

### Encoding
- Nominal representation using `OneHotEncoder`.

---

# Model Benchmarking and Optimization

The architecture dynamically evaluates top-tier regression algorithms on the validation set before committing to heavy optimization.

## Benchmarked Algorithms

- Naive baseline (`DummyRegressor`, always predicts the mean salary): the floor every model must beat
- Ridge Regression
- Random Forest Regressor
- Gradient Boosting Regressor
- XGBoost Regressor
- LightGBM Regressor

---

# Hyperparameter Tuning (Optuna)

The best model from the validation benchmark is selected automatically and injected into an **Optuna** study (TPE sampler, fixed seed) that maximizes the validation **R-squared (R²)** score. Each algorithm has its own search space (e.g. `n_estimators`, `learning_rate`, `max_depth`, `num_leaves`, `subsample`).

In the current run **LightGBM** (validation R² 0.9785) narrowly beat **XGBoost** (0.9782) and was selected for tuning.

The final model is retrained on Train + Validation with the best parameters and evaluated once on the untouched Test set.

---

# Results

### Validation benchmark

| Model | R² | RMSE | MAE |
|---|---|---|---|
| LightGBM | 0.9785 | 5,460 | 4,350 |
| XGBoost | 0.9782 | 5,494 | 4,375 |
| Random Forest | 0.9707 | 6,378 | 5,054 |
| Ridge Regression | 0.9630 | 7,163 | 5,486 |
| Gradient Boosting | 0.9546 | 7,938 | 6,231 |
| Baseline (mean) | 0.0000 | 37,243 | 29,687 |

Optuna raised LightGBM's validation R² from 0.9785 to 0.9803.

### Test set (50,000 unseen records)

| Model | MAE | RMSE | R² | MAPE | Predictions within ±10% |
|---|---|---|---|---|---|
| Tuned LightGBM | 4,129 | 5,176 | 0.9807 | 3.04% | 98.1% |
| Baseline (mean) | 29,695 | 37,281 | 0.0000 | 22.66% | 30.6% |

### Error analysis

The absolute error is almost constant across segments (MAE ≈ 4,000–4,260), so the **relative** error is highest where salaries are lowest: MAPE is 2.4% in the USA vs. 4.5% in India, 2.6% for AI Engineers vs. 3.8% for Data/Business Analysts, and 2.1% in the top salary quintile vs. 4.4% in the bottom one.

### What drives the predictions

Permutation importance (MAE increase when a feature is shuffled, test sample of 10,000): `location` (+18,777), `experience_years` (+15,239), `company_size` (+13,646), `job_title` (+13,489) and `education_level` (+9,603) dominate; `skills_count`, `certifications` and `remote_work` add little, and `industry` has practically no effect (+2).

### Limitations

The dataset shows the hallmarks of synthetic data (near-uniform category shares, no missing values, no industry effect), which explains the very high R². The pipeline is designed to transfer to real compensation data, but the metrics should not be read as real-world salary accuracy.

---

# How to Run

```bash
pip install -r requirements.txt
jupyter notebook "notebooks/Employee Salary Prediction.ipynb"
```

The notebook resolves the dataset path relative to the project, so it runs from the project root or from the `notebooks/` folder.

---

# Visualizations and Diagnostics

The notebook generates diagnostic plots to assess data integrity and model performance: target distribution, salary by categorical feature, benchmark comparison, actual vs. predicted and residual distribution.

---

# Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- XGBoost
- LightGBM
- Optuna
- Matplotlib
- Seaborn

---

# License

Educational Machine Learning project for academic purposes. Dataset: [Job Salary Prediction Dataset](https://www.kaggle.com/datasets/nalisha/job-salary-prediction-dataset) by nalisha on Kaggle.

---

# Author

**Armando Guarnera**  
Data Scientist  
Argentina