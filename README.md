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

## 3. Validación Cruzada y Uso de "Bandas"

Se utilizó `GroupKFold` y `cross_validate` para garantizar la estabilidad del modelo. Un aspecto clave en este análisis fue la **visualización de bandas**:

- **¿Qué representan las bandas?**: En las gráficas de rendimiento (como las de `n_estimators` o `C`), el uso de Seaborn permitió visualizar una **banda sombreada** alrededor de la línea de precisión media.
- **Significado técnico**: Esta banda representa el **intervalo de confianza** (o desviación estándar) de los resultados obtenidos en los diferentes "folds" de la validación cruzada.
- **Impacto en la optimización**:
  - Una banda **estrecha** indicaba que el modelo era estable y consistente independientemente de cómo se dividieran los datos.
  - El análisis de estas bandas permitió confirmar que el modelo no solo era preciso, sino también **robusto**, ya que las variaciones entre grupos de datos eran mínimas.

## 4. Visualización y Herramientas de Análisis

- **Matplotlib y Seaborn**: Esenciales para graficar la evolución de la precisión.
- **Curvas de Aprendizaje**: Permitieron detectar visualmente el punto donde el modelo dejaba de aprender y empezaba a sobreajustarse.
- **Diagramas de Dispersión**: Utilizados para validar cómo el modelo SVC separaba las diferentes clases de pH en el espacio dimensional.

## 5. Conclusión

El proyecto demuestra que mediante el ajuste fino de parámetros en SVC y RandomForest, y la validación rigurosa a través de bandas de confianza, es posible crear un sistema de clasificación de suelos con una fiabilidad cercana al 100%. La estabilidad mostrada por las bandas de validación asegura que el modelo es apto para su aplicación en entornos reales de análisis edafológico.
