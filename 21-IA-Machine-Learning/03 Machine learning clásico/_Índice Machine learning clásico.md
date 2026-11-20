---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA y Machine Learning
tags: [indice, ia-machine-learning]
---
# Machine learning clásico

Parte de: [[21-IA-Machine-Learning/_Índice IA y Machine Learning|IA y Machine Learning]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es el machine learning]]
- [[Aprendizaje supervisado, no supervisado y por refuerzo]]
  - [[Aprendizaje supervisado]]
  - [[Aprendizaje no supervisado]]
  - [[Aprendizaje por refuerzo]]
- [[Datos de entrenamiento, validación y prueba]]
- [[Regresión lineal]]
- [[Regresión logística]]
- [[Árboles de decisión y random forest]]
- [[Gradient boosting (XGBoost y LightGBM)]]
- [[k-NN y SVM]]
- [[Clustering (k-means)]]
- [[Reducción de dimensionalidad (PCA)]]
- [[Overfitting, underfitting y sesgo-varianza]]
- [[Validación cruzada]]
- [[Métricas de clasificación y regresión]]
  - [[Matriz de confusión]]
  - [[Precisión, recall y F1]]
  - [[Curva ROC y AUC]]
  - [[Métricas de regresión (MAE, RMSE y R²)]]
- [[Feature engineering]]
- [[scikit-learn]]

## Laboratorios

- [[Lab 2 - Regresión y clasificación con scikit-learn]]

## Progreso

```base
filters:
  and:
    - file.inFolder("21-IA-Machine-Learning/03 Machine learning clásico")
    - file.name != "_Índice Machine learning clásico"
views:
  - type: table
    name: Temas
    filters:
      and:
        - tipo != "laboratorio"
    order:
      - file.name
      - progreso
      - nivel
  - type: table
    name: Laboratorios
    filters:
      and:
        - tipo == "laboratorio"
    order:
      - file.name
      - progreso
```
