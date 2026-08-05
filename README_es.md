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
| **Atributos** | 10 características |

El conjunto de datos contiene información completa sobre perfiles de empleados y características laborales. Las variables utilizadas en este análisis incluyen:

| Característica | Descripción |
|----------|-------------|
| `job_title` | Cargo o puesto profesional del empleado. |
| `experience_years` | Años de experiencia profesional. |
| `education_level` | Nivel educativo más alto alcanzado (por ejemplo, Licenciatura, Maestría, Doctorado). |
| `skills_count` | Número total de habilidades técnicas o profesionales documentadas. |
| `industry` | Sector en el que opera la empresa. |
| `company_size` | Clasificación del tamaño de la organización según cantidad de empleados o ingresos. |
| `location` | Ubicación geográfica del puesto. |
| `remote_work` | Modalidad de trabajo (por ejemplo, Sí, No, Híbrido). |
| `certifications` | Número de certificaciones profesionales obtenidas. |

---

# Flujo de Trabajo del Proyecto

El proyecto sigue un flujo de trabajo riguroso y profesional de ciencia de datos:

1. **Adquisición y Limpieza de Datos**
   - Ingesta inicial de datos, validación y eliminación de duplicados.

2. **Análisis Exploratorio de Datos (EDA)**
   - Análisis estadístico de la variable objetivo y de las relaciones entre variables categóricas.

3. **División de los Datos**
   - Implementación de una estrategia estricta de separación en Train/Validation/Test.

4. **Transformadores Personalizados y Preprocesamiento**
   - Construcción de un pipeline modular utilizando clases personalizadas de scikit-learn.

5. **Benchmarking Dinámico de Modelos**
   - Evaluación cruzada de múltiples algoritmos para establecer la mejor línea base.

6. **Optimización de Hiperparámetros**
   - Exploración automatizada del espacio de búsqueda utilizando Optuna.

7. **Evaluación Final**
   - Evaluación del mejor modelo sobre datos de prueba no vistos para garantizar su capacidad de generalización.

---

# Preprocesamiento e Ingeniería de Características

El pipeline está completamente modularizado utilizando `scikit-learn Pipelines` y `ColumnTransformer`.

Se desarrollaron clases personalizadas basadas en `BaseEstimator` y `TransformerMixin` para abordar requisitos específicos del dominio.

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

- Ridge Regression
- Random Forest Regressor
- Gradient Boosting Regressor
- XGBoost Regressor
- LightGBM Regressor

---

# Optimización de Hiperparámetros (Optuna)

Basándose en los resultados del benchmarking sobre validación, **XGBoost** fue seleccionado como el algoritmo óptimo.

Se llevó a cabo un proceso automatizado de optimización de hiperparámetros utilizando **Optuna** para maximizar la métrica **R-cuadrado (R²)**.

### Parámetros Optimizados

- `n_estimators`
- `learning_rate`
- `max_depth`
- `subsample`

---

# Visualizaciones y Diagnósticos

El repositorio genera gráficos de diagnóstico automatizados para evaluar la integridad de los datos y el rendimiento del modelo.

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

Proyecto educativo de Machine Learning que utiliza un dataset de Kaggle con fines académicos.

---

# Autor

**Armando Guarnera**  
Data Scientist  
Argentina