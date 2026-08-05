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
| **Attributes** | 10 features |

The dataset contains comprehensive employee profile and job information. The features utilized in this analysis include:

| Feature | Description |
|----------|-------------|
| `job_title` | The professional designation of the employee. |
| `experience_years` | Years of professional experience. |
| `education_level` | Highest educational degree attained (e.g., Bachelor, Master, PhD). |
| `skills_count` | Total number of documented technical or professional skills. |
| `industry` | The sector in which the company operates. |
| `company_size` | Categorization of the organization's employee count or revenue. |
| `location` | Geographical location of the role. |
| `remote_work` | Modality of work (e.g., Yes, No, Hybrid). |
| `certifications` | Number of professional certifications held. |

---

# Project Workflow

The project follows a rigorous, professional data science workflow:

1. **Data Acquisition and Cleaning**
   - Initial data ingestion, validation, and duplication removal.

2. **Exploratory Data Analysis (EDA)**
   - Statistical analysis of the target variable and categorical relationships.

3. **Data Splitting**
   - Implementation of a strict Train/Validation/Test split strategy

4. **Custom Transformers and Preprocessing**
   - Modular pipeline construction utilizing custom scikit-learn classes.

5. **Dynamic Model Benchmarking**
   - Cross-evaluating multiple algorithms to establish the strongest baseline.

6. **Hyperparameter Tuning**
   - Automated search space exploration using Optuna.

7. **Final Evaluation**
   - Assessing the best model on unseen test data to guarantee generalizability.

---

# Preprocessing and Feature Engineering

The pipeline is fully modularized utilizing `scikit-learn Pipelines` and `ColumnTransformer`.

Custom `BaseEstimator` and `TransformerMixin` classes were developed to handle specific domain requirements.

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

Based on the validation benchmarking, **XGBoost** was selected as the optimal algorithm.

An automated hyperparameter optimization process was conducted using **Optuna** to maximize the **R-squared (R²)** score.

### Tuned Parameters

- `n_estimators`
- `learning_rate`
- `max_depth`
- `subsample`

---

# Visualizations and Diagnostics

The repository generates automated diagnostic plots to assess data integrity and model performance

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