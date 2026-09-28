*Leer este documento en otros idiomas: [English](README.md)*

# Proyecto de Machine Learning - Predicción de Salarios de Empleados

Este proyecto desarrolla un pipeline avanzado de regresión de Machine Learning de extremo a extremo, diseñado para predecir salarios de empleados utilizando información profesional, educativa y relacionada con el puesto de trabajo.

El objetivo es construir un sistema escalable y preparado para producción para la estimación de salarios mediante un sólido preprocesamiento de datos, ingeniería de características personalizada, benchmarking dinámico de modelos y optimización automatizada de hiperparámetros.

---

# Acerca del Dataset

| Atributo | Descripción |
|------------|------------|
| **Nombre** | Employee Salary Prediction Dataset |
| **Variable Objetivo** | `salary` |
| **Tipo de Archivo** | CSV |
| **Volumen** | 250.000 registros |
| **Atributos** | 9 características + 1 variable objetivo |

El conjunto de datos contiene información completa sobre perfiles de empleados y características laborales. Las variables utilizadas en este análisis incluyen:

| Característica | Descripción |
|----------|-------------|
| `job_title` | Cargo o puesto profesional del empleado. |
| `experience_years` | Años de experiencia profesional. |
| `education_level` | Nivel educativo más alto alcanzado (High School, Diploma, Bachelor, Master, PhD). |
| `skills_count` | Número total de habilidades técnicas o profesionales documentadas. |
| `industry` | Sector en el que opera la empresa. |
| `company_size` | Tamaño de la organización (Startup, Small, Medium, Large, Enterprise). |
| `location` | Ubicación geográfica del puesto. |
| `remote_work` | Modalidad de trabajo (Yes, No, Hybrid). |
| `certifications` | Número de certificaciones profesionales obtenidas. |

---

# Flujo de Trabajo del Proyecto

El proyecto sigue un flujo de trabajo riguroso y profesional de ciencia de datos:

1. **Adquisición y Limpieza de Datos**
   - Ingesta inicial de datos, validación y eliminación de duplicados.

2. **Análisis Exploratorio de Datos (EDA)**
   - Análisis estadístico de la variable objetivo y de las relaciones entre variables categóricas.

3. **División de los Datos**
   - Separación estricta en Train/Validation/Test (64% / 16% / 20%).

4. **Transformadores Personalizados y Preprocesamiento**
   - Construcción de un pipeline modular utilizando clases personalizadas de scikit-learn.

5. **Benchmarking Dinámico de Modelos**
   - Comparación de múltiples algoritmos sobre el conjunto de validación para establecer la mejor línea base.

6. **Optimización de Hiperparámetros**
   - Exploración automatizada del espacio de búsqueda utilizando Optuna.

7. **Evaluación Final**
   - Evaluación del mejor modelo sobre datos de prueba no vistos para garantizar su capacidad de generalización.

8. **Análisis de Errores e Interpretabilidad**
   - Desglose de errores por segmento y medición de qué variables impulsan las predicciones (importancia por permutación).

---

# Preprocesamiento e Ingeniería de Características

El pipeline está completamente modularizado utilizando `scikit-learn Pipelines` y `ColumnTransformer`.

Se desarrollaron clases personalizadas basadas en `BaseEstimator` y `TransformerMixin` para abordar requisitos específicos del dominio.

## Ingeniería de Características

- Transformador personalizado `FeatureEngineer` que agrega rangos ordinales para `education_level` y `company_size`, de modo que el orden natural de estas categorías esté disponible para los modelos junto con su codificación one-hot.

## Pipeline Numérico

### Tratamiento de Valores Faltantes
- Imputación mediante la mediana.

### Tratamiento de Valores Atípicos
- Transformador personalizado `IQRTransformer` que aplica clipping basado en el método del Rango Intercuartílico (IQR) con un factor de 1.5.

### Escalado
- Estandarización utilizando `StandardScaler`.

## Pipeline Categórico

### Tratamiento de Valores Faltantes
- Imputación mediante la categoría más frecuente.

### Reducción de Dimensionalidad
- Transformador personalizado `RareCategoryEncoder` que agrupa categorías poco frecuentes (umbral < 5%) dentro de una categoría unificada denominada `"Other"`.

### Codificación
- Representación nominal mediante `OneHotEncoder`.

---

# Benchmarking y Optimización de Modelos

La arquitectura evalúa dinámicamente algoritmos de regresión de alto rendimiento sobre el conjunto de validación antes de realizar procesos intensivos de optimización.

## Algoritmos Evaluados

- Línea base ingenua (`DummyRegressor`, predice siempre el salario medio): el piso que todo modelo debe superar
- Ridge Regression
- Random Forest Regressor
- Gradient Boosting Regressor
- XGBoost Regressor
- LightGBM Regressor

---

# Optimización de Hiperparámetros (Optuna)

El mejor modelo del benchmarking sobre validación se selecciona automáticamente y se optimiza con un estudio de **Optuna** (sampler TPE con semilla fija) que maximiza la métrica **R-cuadrado (R²)** en validación. Cada algoritmo tiene su propio espacio de búsqueda (por ejemplo `n_estimators`, `learning_rate`, `max_depth`, `num_leaves`, `subsample`).

En la ejecución actual **LightGBM** (R² de validación 0.9785) superó por muy poco a **XGBoost** (0.9782) y fue seleccionado para la optimización.

El modelo final se reentrena sobre Train + Validation con los mejores parámetros y se evalúa una única vez sobre el conjunto de Test.

---

# Resultados

### Benchmark en validación

| Modelo | R² | RMSE | MAE |
|---|---|---|---|
| LightGBM | 0,9785 | 5.460 | 4.350 |
| XGBoost | 0,9782 | 5.494 | 4.375 |
| Random Forest | 0,9707 | 6.378 | 5.054 |
| Ridge Regression | 0,9630 | 7.163 | 5.486 |
| Gradient Boosting | 0,9546 | 7.938 | 6.231 |
| Línea base (media) | 0,0000 | 37.243 | 29.687 |

Optuna elevó el R² de validación de LightGBM de 0,9785 a 0,9803.

### Conjunto de Test (50.000 registros no vistos)

| Modelo | MAE | RMSE | R² | MAPE | Predicciones dentro de ±10% |
|---|---|---|---|---|---|
| LightGBM optimizado | 4.129 | 5.176 | 0,9807 | 3,04% | 98,1% |
| Línea base (media) | 29.695 | 37.281 | 0,0000 | 22,66% | 30,6% |

### Análisis de errores

El error absoluto es casi constante entre segmentos (MAE ≈ 4.000–4.260), por lo que el error **relativo** es mayor donde los salarios son más bajos: el MAPE es 2,4% en USA frente a 4,5% en India, 2,6% para AI Engineers frente a 3,8% para Data/Business Analysts, y 2,1% en el quintil salarial superior frente a 4,4% en el inferior.

### Qué impulsa las predicciones

Importancia por permutación (aumento del MAE al desordenar una variable, muestra de test de 10.000): `location` (+18.777), `experience_years` (+15.239), `company_size` (+13.646), `job_title` (+13.489) y `education_level` (+9.603) dominan; `skills_count`, `certifications` y `remote_work` aportan poco, e `industry` prácticamente no tiene efecto (+2).

### Limitaciones

El dataset muestra rasgos típicos de datos sintéticos (proporciones de categorías casi uniformes, sin valores faltantes, sin efecto de la industria), lo que explica el R² tan alto. El pipeline está diseñado para trasladarse a datos reales de compensación, pero las métricas no deben leerse como precisión salarial en el mundo real.

---

# Cómo Ejecutarlo

```bash
pip install -r requirements.txt
jupyter notebook "notebooks/Employee Salary Prediction.ipynb"
```

El notebook resuelve la ruta del dataset de forma relativa al proyecto, por lo que funciona desde la raíz o desde la carpeta `notebooks/`.

---

# Visualizaciones y Diagnósticos

El notebook genera gráficos de diagnóstico para evaluar la integridad de los datos y el rendimiento del modelo: distribución de la variable objetivo, salario por variable categórica, comparación del benchmark, valores reales vs. predichos y distribución de residuos.

---

# Tecnologías Utilizadas

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

# Licencia

Proyecto educativo de Machine Learning con fines académicos. Dataset: [Job Salary Prediction Dataset](https://www.kaggle.com/datasets/nalisha/job-salary-prediction-dataset) de nalisha en Kaggle.

---

# Autor

**Armando Guarnera**  
Data Scientist  
Argentina