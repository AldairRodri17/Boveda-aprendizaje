---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: IA y Machine Learning
herramienta: Google Colab o Jupyter local con scikit-learn
tags: [ia-machine-learning, laboratorio]
---
# Lab 2 - Regresión y clasificación con scikit-learn

Parte de: [[21-IA-Machine-Learning/03 Machine learning clásico/_Índice Machine learning clásico|Machine learning clásico]]

**Cubre:** [[scikit-learn]] · [[Regresión lineal]] · [[Regresión logística]] · [[Árboles de decisión y random forest]] · [[Datos de entrenamiento, validación y prueba]] · [[Validación cruzada]] · [[Métricas de clasificación y regresión]] · [[Matriz de confusión]] · [[Precisión, recall y F1]] · [[Curva ROC y AUC]] · [[Métricas de regresión (MAE, RMSE y R²)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Entrenar y evaluar correctamente un modelo de regresión y uno de clasificación con datasets incluidos en scikit-learn, usando pipelines, validación cruzada y las métricas adecuadas.

## Pasos

1. **Regresión**: carga `fetch_california_housing(as_frame=True)`. Divide 80/20 con `random_state=42`.
2. Entrena un modelo base que prediga siempre la media (`DummyRegressor`) y una `LinearRegression` dentro de un pipeline con `StandardScaler`. Reporta MAE, RMSE y R² en prueba.
3. Entrena un `RandomForestRegressor` y compara.
4. **Clasificación**: carga `load_breast_cancer()`. Divide 80/20 estratificado.
5. Crea un pipeline `StandardScaler` + `LogisticRegression` y evalúalo con validación cruzada de 5 folds (métrica F1) solo sobre entrenamiento.
6. Ajusta en entrenamiento, predice en prueba y muestra matriz de confusión, `classification_report` y ROC AUC.
7. Cambia el umbral de decisión a 0.3 y observa cómo cambian precisión y recall de la clase de interés.
8. Escribe en el notebook qué modelo elegirías en cada caso y por qué.

## Criterios de terminado

- [ ] La regresión lineal supera al `DummyRegressor` en MAE y su R² en prueba es mayor que 0.
- [ ] Se reporta la media y desviación de F1 de los 5 folds.
- [ ] Se muestra la matriz de confusión de prueba con ejes rotulados.
- [ ] Se reporta ROC AUC en prueba usando `predict_proba`, no `predict`.
- [ ] El escalador nunca se ajusta con datos de prueba (todo va dentro del pipeline).
- [ ] Hay una comparación escrita de precisión y recall con umbral 0.5 vs 0.3.

## Solución

> [!success]- Regresión
> ```python
> import numpy as np
> from sklearn.datasets import fetch_california_housing
> from sklearn.model_selection import train_test_split
> from sklearn.pipeline import make_pipeline
> from sklearn.preprocessing import StandardScaler
> from sklearn.linear_model import LinearRegression
> from sklearn.dummy import DummyRegressor
> from sklearn.ensemble import RandomForestRegressor
> from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
>
> X, y = fetch_california_housing(return_X_y=True, as_frame=True)
> X_tr, X_te, y_tr, y_te = train_test_split(X, y, test_size=0.2, random_state=42)
> modelos = {
>     'base': DummyRegressor(),
>     'lineal': make_pipeline(StandardScaler(), LinearRegression()),
>     'bosque': RandomForestRegressor(n_estimators=200, random_state=42, n_jobs=-1),
> }
> for nombre, m in modelos.items():
>     p = m.fit(X_tr, y_tr).predict(X_te)
>     print(nombre, mean_absolute_error(y_te, p), np.sqrt(mean_squared_error(y_te, p)), r2_score(y_te, p))
> ```
> Esperado: el bosque suele ganar a la lineal, y ambas al modelo base (R² ≈ 0).

> [!success]- Clasificación
> ```python
> from sklearn.datasets import load_breast_cancer
> from sklearn.linear_model import LogisticRegression
> from sklearn.model_selection import cross_val_score
> from sklearn.metrics import classification_report, roc_auc_score, ConfusionMatrixDisplay, precision_score, recall_score
>
> X, y = load_breast_cancer(return_X_y=True)
> X_tr, X_te, y_tr, y_te = train_test_split(X, y, test_size=0.2, stratify=y, random_state=42)
> pipe = make_pipeline(StandardScaler(), LogisticRegression(max_iter=1000))
> s = cross_val_score(pipe, X_tr, y_tr, cv=5, scoring='f1'); print(s.mean(), s.std())
> pipe.fit(X_tr, y_tr)
> print(classification_report(y_te, pipe.predict(X_te)))
> proba = pipe.predict_proba(X_te)[:, 1]
> print('AUC', roc_auc_score(y_te, proba))
> ConfusionMatrixDisplay.from_estimator(pipe, X_te, y_te)
> for u in (0.5, 0.3):
>     pred = (proba >= u).astype(int)
>     print(u, precision_score(y_te, pred), recall_score(y_te, pred))
> ```
> En este dataset la clase `1` es "benigno"; si te interesa detectar malignos, evalúa la clase `0` (`pos_label=0`) y mueve el umbral sobre `proba` de esa clase.

## Notas personales

_Qué me costó, errores que cometí, dudas._
