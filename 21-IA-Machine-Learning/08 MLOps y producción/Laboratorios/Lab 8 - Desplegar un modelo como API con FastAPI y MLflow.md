---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: IA y Machine Learning
herramienta: Python local con scikit-learn, MLflow, FastAPI y uvicorn (Docker opcional)
tags: [ia-machine-learning, laboratorio]
contenido: completo
---
# Lab 8 - Desplegar un modelo como API con FastAPI y MLflow

Parte de: [[21-IA-Machine-Learning/08 MLOps y producción/_Índice MLOps y producción|MLOps y producción]]

**Cubre:** [[Desplegar un modelo como API]] · [[MLflow]] · [[Versionado de datos y modelos]] · [[MLOps - conceptos]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Registrar experimentos con MLflow, guardar el mejor pipeline y servirlo como API REST validada con FastAPI.

## Pasos

1. Crea un entorno virtual e instala `scikit-learn mlflow fastapi uvicorn joblib`.
2. Entrena 3 variantes del pipeline del Lab 2 (cáncer de mama) con distintos `C` y registra en MLflow parámetros, F1 y el modelo.
3. Abre `mlflow ui` y compara las corridas.
4. Guarda el mejor pipeline con `joblib.dump`.
5. Crea `app.py` con FastAPI: carga el modelo al iniciar, define un esquema Pydantic con 30 valores y un endpoint `POST /predict` que devuelva clase y probabilidad.
6. Arranca con `uvicorn app:app --reload` y prueba desde `/docs`.
7. Envía una petición con 29 valores y verifica que la API responde 422 (validación).
8. Opcional: empaqueta en un Dockerfile.

## Criterios de terminado

- [ ] `mlflow ui` muestra 3 corridas con el parámetro `C` y la métrica `f1`.
- [ ] `POST /predict` con 30 valores devuelve 200 con `clase` y `probabilidad` entre 0 y 1.
- [ ] Una petición con 29 valores devuelve 422.
- [ ] El modelo se carga una sola vez al iniciar (no en cada petición).

## Solución

> [!success]- Entrenamiento con MLflow
> ```python
> import mlflow, mlflow.sklearn, joblib
> from sklearn.datasets import load_breast_cancer
> from sklearn.model_selection import train_test_split
> from sklearn.pipeline import make_pipeline
> from sklearn.preprocessing import StandardScaler
> from sklearn.linear_model import LogisticRegression
> from sklearn.metrics import f1_score
>
> X, y = load_breast_cancer(return_X_y=True)
> X_tr, X_te, y_tr, y_te = train_test_split(X, y, test_size=0.2, stratify=y, random_state=42)
> mejor = (0, None)
> for C in (0.01, 0.1, 1.0):
>     with mlflow.start_run():
>         pipe = make_pipeline(StandardScaler(), LogisticRegression(C=C, max_iter=1000)).fit(X_tr, y_tr)
>         f1 = f1_score(y_te, pipe.predict(X_te))
>         mlflow.log_param('C', C); mlflow.log_metric('f1', f1)
>         mlflow.sklearn.log_model(pipe, 'modelo')
>         mejor = max(mejor, (f1, pipe), key=lambda t: t[0])
> joblib.dump(mejor[1], 'modelo.joblib')
> ```
> En un proyecto real elige el mejor con un conjunto de validación, no con el de prueba.

> [!success]- API con FastAPI
> ```python
> # app.py
> import joblib, numpy as np
> from fastapi import FastAPI
> from pydantic import BaseModel, Field
>
> modelo = joblib.load('modelo.joblib')     # una vez al arrancar
> app = FastAPI()
>
> class Entrada(BaseModel):
>     valores: list[float] = Field(min_length=30, max_length=30)
>
> @app.post('/predict')
> def predict(e: Entrada):
>     p = modelo.predict_proba(np.array([e.valores]))[0]
>     return {'clase': int(p.argmax()), 'probabilidad': float(p.max())}
> ```
> Con 29 valores Pydantic rechaza la petición y FastAPI responde 422.

## Notas personales

_Qué me costó, errores que cometí, dudas._
