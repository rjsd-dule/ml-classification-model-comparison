# ml-classification-model-comparison

Este proyecto explora y compara múltiples algoritmos de clasificación, incluyendo DecisionTreeClassifier, RandomForestClassifier, Support Vector Classifier (SVC) y MLPClassifier. El objetivo es evaluar su rendimiento y determinar qué modelo obtiene la mayor precisión al ser entrenado con un dataset.

# Informe de Desarrollo: Proyecto de Clasificación de pH del Suelo

Este informe documenta el proceso de desarrollo, optimización y evaluación de modelos de Machine Learning para la clasificación del estado del pH del suelo.

## 1. Proceso de Limpieza de Datos

El conjunto de datos presentaba valores faltantes en columnas críticas. El proceso de limpieza aseguró la calidad del entrenamiento mediante:

- **Identificación de nulos**: Localización de valores faltantes en `PH`, `STATE_PH` y `STATE_NUMBER`.
- **Eliminación de registros incompletos**: Uso de `dropna` para limpiar las características y etiquetas principales.
- **Transformación y Ordenamiento**: Se generó la columna `STATE_PH_INT` (etiquetas numéricas) y se ordenaron los datos por valor de pH para analizar mejor las transiciones entre categorías.

## 2. Soporte de Vectores (SVC) y Ajuste de Hiperparámetros

El modelo **SVC (Support Vector Classifier)** fue una pieza central en la comparación de rendimiento. El proceso de optimización incluyó:

- **Exploración de Kernels**: Se evaluaron los tipos `linear`, `rbf` y `poly`. El kernel **rbf** demostró ser el más eficaz para capturar las relaciones no lineales entre el valor numérico del pH y su categoría.
- **Optimización del parámetro C**: Se realizó una búsqueda iterativa (rango 1-27) para el parámetro de regularización. Se identificó que un **C = 4** con kernel **rbf** lograba un equilibrio óptimo, alcanzando un **99.35% de precisión** en pruebas.
- **Grados en Polinomios**: Para el kernel `poly`, se probaron grados 2 y 3, observando una alta capacidad de ajuste pero con mayor costo computacional.

## 3. Validación Cruzada y Representación Gráfica

Se utilizó `GroupKFold` y `cross_validate` para garantizar la estabilidad del modelo. La evaluación del rendimiento se apoyó en una sólida infraestructura de visualización:

- **Estructura de Gráficos con Matplotlib**: Se empleó la función `add_subplot` de la librería **Matplotlib** para organizar las métricas de rendimiento en una cuadrícula comparativa. Esto permitió analizar simultáneamente el comportamiento de entrenamiento y prueba bajo distintos parámetros.
- **Visualización de la Varianza**: En lugar de simples líneas, los gráficos incluyen áreas sombreadas generadas mediante el análisis de validación cruzada. Estas áreas representan el **intervalo de confianza** (o desviación estándar) de los resultados en los diferentes "folds".
- **Análisis de Estabilidad**:
  - Un área sombreada **reducida** en el subplot indica que el modelo es estable y consistente, con poca variabilidad ante diferentes divisiones de los datos.
  - El uso de estos gráficos permitió confirmar que el modelo no solo era preciso, sino también **robusto**, validando visualmente la convergencia entre los datos de entrenamiento y validación.

## 4. Visualización y Herramientas de Análisis

- **Matplotlib**: Se utilizó principalmente para la gestión de figuras y la creación de sub-gráficos (`add_subplot`), permitiendo una comparación técnica detallada entre modelos.
- **Seaborn**: Complementó a Matplotlib para la representación estadística, facilitando la visualización de las tendencias de precisión y sus intervalos de confianza.
- **Curvas de Aprendizaje**: Integradas en los subplots para detectar visualmente el punto de equilibrio óptimo y prevenir el sobreajuste (overfitting).

## 5. Comparativa de Modelos y Conclusión

Tras evaluar los diferentes algoritmos, se presenta la siguiente comparativa de rendimiento basada en las métricas finales obtenidas:

| Métrica | DecisionTree | RandomForest | SVC | MLPClassifier |
| :--- | :---: | :---: | :---: | :---: |
| **Accuracy** | 99.2% | 95.92% | **100.0%** | 99.70% |
| **Precision** | 97.77% | 81.43% | **100.0%** | 99.25% |
| **Recall** | 98.87% | 85.71% | **100.0%** | 99.31% |
| **F1-Score** | 98.2% | 83.19% | **100.0%** | 99.28% |

### Conclusión Final
El modelo **SVC (Support Vector Classifier)** demostró el rendimiento más sobresaliente, alcanzando una precisión y estabilidad del **100%** tras la optimización del parámetro **C=4** y el uso del kernel **rbf**. Esta superioridad se debe a su capacidad para definir fronteras de decisión precisas en el espacio dimensional de los valores de pH.

El éxito del proyecto radica en:
1.  El ajuste fino de hiperparámetros, especialmente en SVC y MLPClassifier.
2.  La validación rigurosa a través de **intervalos de confianza** visualizados en subplots mediante `add_subplot` de **Matplotlib**.
3.  La estabilidad mostrada por los **gráficos de validación**, que asegura que el modelo es altamente fiable y apto para su aplicación en entornos reales de análisis edafológico.
