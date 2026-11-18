<img width="1280" height="640" alt="WhatsApp Image 2026-11-17 at 3 00 47 PM" src="https://github.com/user-attachments/assets/b42e24a0-68a5-4dc6-99ae-bf821d6cac08" />

# 📡 Σ Proyecto Sigma | Predicción de Fuga de Clientes en Telecomunicaciones (Customer Churn)

## 📋 Descripción
La fuga de clientes (*Customer Churn*) representa uno de los principales desafíos para las empresas de telecomunicaciones, ya que impacta directamente los ingresos recurrentes, la estabilidad de la cartera y los costos de adquisición de nuevos usuarios.

Este proyecto desarrolla un sistema de Machine Learning de extremo a extremo capaz de predecir qué clientes presentan mayor probabilidad de abandonar la compañía, integrando información contractual, demográfica, de servicios contratados y de facturación para respaldar estrategias proactivas de retención y fidelización.

## 🎯 Objetivos
- ✅ Integrar y depurar múltiples fuentes de datos relacionales mediante `customerID` (`outer join`), consolidando una base de **7.043 clientes**.
- ✅ Realizar un análisis exploratorio (EDA) para identificar los principales impulsores contractuales, de servicio y de facturación asociados al abandono.
- ✅ Prevenir la fuga de datos (*data leakage*), codificar variables categóricas (*One-Hot Encoding*) y equilibrar el conjunto de entrenamiento mediante sobremuestreo (`resample`).
- ✅ Entrenar, comparar y seleccionar el mejor algoritmo de clasificación evaluando **ROC-AUC** y **Sensibilidad (Recall)** para maximizar la detección de clientes en riesgo.

## 🛠️ Herramientas y Habilidades Utilizadas
* **Lenguaje:** Python
* **Librerías:** Pandas, NumPy, Matplotlib, Seaborn, Scikit-Learn, LightGBM y XGBoost
* **Entorno:** Jupyter Notebook (`Proyecto_Sigma_Prediccion_de_Fuga_de_Clientes.ipynb`)
* **Técnicas:** Integración Relacional de Datos, Análisis Exploratorio (EDA), One-Hot Encoding, Balanceo de Clases (*Oversampling*), Clasificación Binaria y Optimización por ROC-AUC / Recall.

## 📊 Fuente de Datos y Estructura
El proyecto consolida información proveniente de cuatro tablas vinculadas por `customerID` (**7.043 registros únicos**):
* **`contract.csv`**: Información contractual, fechas de inicio/cancelación, método de pago y cargos mensuales/totales.
* **`personal.csv`**: Datos demográficos del titular de la cuenta.
* **`internet.csv`**: Servicios adicionales contratados (seguridad en línea, soporte técnico, respaldo, streaming).
* **`phone.csv`**: Detalles de líneas telefónicas contratadas.

## ⚙️ Metodología
1. **Preparación e Integración de Datos:** Fusión de tablas por `customerID` mediante `outer join`, imputación semántica según reglas de negocio, creación de la variable objetivo binaria (`Fuga`), exclusión de atributos con riesgo de *data leakage*, codificación *One-Hot Encoding* y aplicación de *Oversampling* (`resample`) exclusivamente sobre el conjunto de entrenamiento.
2. **Análisis Exploratorio de Datos (EDA):** Evaluación del impacto de variables clave como el método de pago (`PaymentMethod`, 45.29 pp de diferencia), tipo de contrato (`Type`, 42.71 pp), servicios de internet (`InternetService`, `OnlineSecurity`, `TechSupport`) y facturación (`MonthlyCharges`, `TotalCharges`).
3. **Modelado y Comparación:** Entrenamiento y validación de seis enfoques: **DummyClassifier** (línea base), **Árbol de Decisión**, **Regresión Logística**, **Bosque Aleatorio**, **LightGBM** y **XGBoost**, priorizando **ROC-AUC** y **Sensibilidad (Recall)**.
4. **Evaluación Final:** Validación del modelo seleccionado (**LightGBM**) sobre datos no vistos en el conjunto de prueba.

## 💡 Resultados y Métricas

### Comparación de Modelos en Validación
| Modelo Evaluado | ROC-AUC | Sensibilidad (Recall) |
| :--- | :---: | :---: |
| **DummyClassifier (Línea Base)** | 0.5000 | 0.0000 |
| **LightGBM (Seleccionado)** | **0.8450** | 0.7433 |
| **Regresión Logística** | 0.8349 | 0.7754 |
| **Bosque Aleatorio (Random Forest)** | 0.8283 | 0.8316 |
| **Árbol de Decisión** | 0.7890 | **0.8797** |
| **XGBoost** | 0.7210 | 0.6043 |

### 🏆 Evaluación Final en el Conjunto de Prueba (`LightGBM`)
| Métrica de Desempeño | Resultado en Prueba |
| :--- | :---: |
| **ROC-AUC** | **0.8492** |
| **Sensibilidad (Recall)** | **0.7781** |

## 📈 Principales Hallazgos
* **Factores críticos de abandono:** El método de pago (`Electronic check` con 45.29% de fuga), el tipo de contrato (`Month-to-month` con 42.71% de fuga) y los cargos mensuales/totales concentran gran parte del poder explicativo del abandono.
* **Equilibrio predictivo de LightGBM:** Mientras el **Árbol de Decisión** alcanzó la mayor sensibilidad aislada (`87.97%`), **LightGBM** ofreció la mejor capacidad discriminativa global tanto en validación (`ROC-AUC = 0.8450`) como en prueba (`ROC-AUC = 0.8492`), identificando correctamente al **77.8%** de los usuarios que abandonan el servicio.
* **Superación de la línea base:** Todos los modelos entrenados superaron ampliamente al clasificador de referencia, demostrando alta estabilidad y generalización sobre datos nuevos.

## 📂 Archivos Principales
* `Proyecto_Sigma_Prediccion_de_Fuga_de_Clientes.ipynb`
* `contract.csv`
* `internet.csv`
* `personal.csv`
* `phone.csv`

## 🏁 Conclusión
Se desarrolló un sistema de clasificación robusto basado en **LightGBM** que alcanza un **ROC-AUC de 0.8492** y una **sensibilidad de 0.7781** sobre datos no vistos. Esta solución proporciona una herramienta accionable para anticipar la deserción de clientes en telecomunicaciones, permitiendo focalizar campañas de fidelización y optimizar la inversión comercial de retención.
