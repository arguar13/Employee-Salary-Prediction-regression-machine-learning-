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

| Model (validation benchmark) | R² | RMSE |
|---|---|---|
| LightGBM | 0.9785 | 5,460 |
| XGBoost | 0.9782 | 5,494 |
| Random Forest | 0.9707 | 6,378 |
| Ridge Regression | 0.9630 | 7,163 |
| Gradient Boosting | 0.9546 | 7,938 |

**Final tuned LightGBM on the Test set:** R² = **0.9807**, RMSE = **5,176**, MAE = **4,129** (mean salary ≈ 145,700).

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

Educational Machine Learning project using a Kaggle dataset for academic purposes.

---

# Author

**Armando Guarnera**  
Data Scientist  
Argentina