<img width="1280" height="640" alt="WhatsApp Image 2026-11-17 at 3 00 47 PM" src="https://github.com/user-attachments/assets/b42e24a0-68a5-4dc6-99ae-bf821d6cac08" />

# 📡 Σ Proyecto Sigma | Predicción de Fuga de Clientes en Telecomunicaciones (Interconnect)

## 📋 Descripción
El operador de telecomunicaciones **Interconnect** busca reducir la tasa de cancelación de usuarios (*Customer Churn*) mediante un sistema predictivo capaz de identificar con anticipación a los clientes con alta probabilidad de darse de baja, permitiendo ofrecerles incentivos y planes especiales de retención.

Este proyecto desarrolla un pipeline completo de Machine Learning de extremo a extremo que integra datos contractuales, demográficos, de servicios de internet y telefonía, ingeniería de características temporales, escalado, ajuste de pesos por desbalance de clases y optimización de hiperparámetros.

## 🎯 Objetivos
- ✅ Integrar las 4 fuentes de datos relacionales mediante `customerID` (left joins), consolidando una base de **7.043 clientes** y gestionando valores nulos estructurales.
- ✅ Realizar ingeniería de características transformando fechas de inicio y fin (`BeginDate`, `EndDate`) en la antigüedad del cliente en días (`ClientAge`) y codificando atributos categóricos con *One-Hot Encoding*.
- ✅ Entrenar y comparar modelos base (**Regresión Logística**, **Bosque Aleatorio** y **LightGBM**) sin balanceo inicial.
- ✅ Optimizar el modelo líder mediante búsqueda de hiperparámetros y ponderación de clases (`scale_pos_weight`) para maximizar **ROC-AUC** y **Sensibilidad (Recall)** sobre datos no vistos.

## 🛠️ Herramientas y Habilidades Utilizadas
* **Lenguaje:** Python
* **Librerías:** Pandas, NumPy, Scikit-Learn (`StandardScaler`, `LogisticRegression`, `RandomForestClassifier`) y LightGBM (`LGBMClassifier`)
* **Entorno:** Jupyter Notebook (`Proyecto_Sigma_Prediccion_de_Fuga_de_Clientes.ipynb`)
* **Técnicas:** Integración Relacional de Datos, Ingeniería de Características Temporales, One-Hot Encoding, Estandarización, Manejo de Desbalance (`scale_pos_weight`) y Búsqueda de Hiperparámetros.

## 📊 Fuente de Datos y Estructura
El conjunto consolidado integra cuatro tablas vinculadas por `customerID` (**7.043 registros** y **22 variables predictoras** tras preprocesamiento):
* **`contract.csv`**: Información contractual, fechas de inicio/cancelación, método de pago y cargos mensuales/totales.
* **`personal.csv`**: Datos demográficos del cliente (`gender`, `SeniorCitizen`, `Partner`, `Dependents`).
* **`internet.csv`**: Servicios de internet contratados (`InternetService`, `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies`).
* **`phone.csv`**: Contratación de líneas telefónicas múltiples (`MultipleLines`).
* **Distribución de la variable objetivo (`Churn`):** 5.174 clientes activos (`0`) frente a 1.869 cancelaciones (`1`), divididos de forma estratificada en entrenamiento (`4.225`), validación (`1.409`) y prueba (`1.409`).

## ⚙️ Metodología
1. **Preparación e Ingeniería de Características:** Fusión por `customerID`, imputación semántica (`'No'`) en servicios no contratados, cálculo de antigüedad del cliente en días (`ClientAge`), codificación *One-Hot Encoding* (`drop_first=True`) y estandarización (`StandardScaler`) de `MonthlyCharges`, `TotalCharges` y `ClientAge`.
2. **Modelado Baseline (Sin Balanceo):** Entrenamiento y comparación inicial en validación de **Regresión Logística**, **Bosque Aleatorio** y **LightGBM**.
3. **Optimización y Manejo de Desbalance:** Búsqueda en malla de hiperparámetros (`learning_rate`, `n_estimators`, `max_depth`, `num_leaves`) sobre **LightGBM**, incorporando ponderación de clase minoritaria (`scale_pos_weight = 5174 / 1869`).
4. **Evaluación Final:** Validación del modelo optimizado sobre el conjunto de prueba aislado (`1.409` clientes).

## 💡 Resultados y Métricas

### Comparación de Modelos en Validación
| Modelo Evaluado | Configuración | ROC-AUC | Sensibilidad (Recall) |
| :--- | :--- | :---: | :---: |
| **Regresión Logística** | Baseline (`max_iter=1000`) | 0.8356 | 0.5802 |
| **Bosque Aleatorio (`RandomForest`)** | Baseline | 0.8336 | 0.5348 |
| **LightGBM (`LGBMClassifier`)** | Baseline | 0.8754 | 0.6096 |
| **LightGBM Optimizado** | `lr=0.1`, `n_est=200`, `depth=5`, `leaves=31`, `scale_pos_weight` | **0.8875** | — |

### 🏆 Evaluación Final en el Conjunto de Prueba (`LightGBM Optimizado`)
| Métrica de Desempeño | Resultado en Prueba |
| :--- | :---: |
| **ROC-AUC Final** | **0.8909** |
| **Sensibilidad (Recall - Clase Churn)** | **0.7674** |
| **Precisión (Clase Churn)** | 0.6600 |
| **Exactitud Global (Accuracy)** | **0.8300** |

## 📈 Principales Hallazgos
* **Impacto de la antigüedad del cliente (`ClientAge`):** Transformar las fechas contractuales en días de permanencia aportó una señal predictiva clave para distinguir abandonos tempranos frente a clientes consolidados.
* **Superioridad de LightGBM y ajuste de pesos:** Mientras los modelos baseline ofrecían un Recall moderado (`53%–61%`) debido al desbalance de clases (~26.5% de fuga), la combinación de ajuste de hiperparámetros y `scale_pos_weight` elevó la detección de fugas al **76.74%** manteniendo un **ROC-AUC de 0.8909**.
* **Valor operativo para Marketing:** De cada 100 clientes con intención real de cancelar el servicio, el sistema logra identificar anticipadamente a **77**, habilitando campañas focalizadas de retención antes de que se concrete la baja.

## 📂 Archivos Principales
* `Proyecto_Sigma_Prediccion_de_Fuga_de_Clientes.ipynb`

## 🏁 Conclusión
Se desarrolló un sistema de clasificación de alta precisión basado en **LightGBM Optimizado** para **Interconnect**, alcanzando un **ROC-AUC de 0.8909** y un **Recall de 0.7674** en prueba. La solución supera la meta estratégica de discriminación (`ROC-AUC > 0.88`) y entrega al equipo comercial una herramienta efectiva para reducir la tasa de abandono y proteger los ingresos recurrentes.
