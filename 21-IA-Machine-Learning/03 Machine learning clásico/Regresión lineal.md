---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: IA y Machine Learning
tags: [ia-machine-learning]
contenido: completo
---
# Regresión lineal

Parte de: [[21-IA-Machine-Learning/03 Machine learning clásico/_Índice Machine learning clásico|Machine learning clásico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La regresión lineal predice un **número** como una **suma ponderada de las entradas** más una constante: `ŷ = w₁x₁ + w₂x₂ + … + b`. Con una sola variable es la recta que mejor pasa entre los puntos. Es el modelo más simple de [[Aprendizaje supervisado]], muy interpretable y sorprendentemente útil: estimar el precio de un departamento por metros y zona, el tiempo de entrega por distancia, o la venta según la inversión en publicidad. Además es la base conceptual de la [[Regresión logística]] y de cada neurona de una red.

### Cómo funciona
**Intuición.** Buscas la recta (o el plano, con más variables) que deje los **errores más pequeños**. El error de cada punto es la distancia vertical entre el valor real y la predicción (el *residuo*). Como los errores positivos y negativos se cancelarían, se elevan al cuadrado: se minimiza el **error cuadrático medio** `MSE = (1/n) Σ (yᵢ − ŷᵢ)²`. Elevar al cuadrado castiga mucho los errores grandes.

**Fórmula mínima (una variable).** Por mínimos cuadrados:
- pendiente `w = Σ(xᵢ − x̄)(yᵢ − ȳ) / Σ(xᵢ − x̄)²`
- intercepto `b = ȳ − w·x̄`

Para los puntos `(1,2), (2,4), (3,5)`: `x̄ = 2`, `ȳ ≈ 3.667`, `w = 3/2 = 1.5`, `b ≈ 0.667`. La recta es `ŷ = 1.5x + 0.667` y predice `≈ 6.667` para `x = 4`.

Con muchas variables la solución tiene forma cerrada (ecuaciones normales) o se encuentra con descenso de gradiente ([[Cálculo para ML (derivadas y gradiente)]]); la pérdida es convexa, así que el mínimo es único.

**Interpretar coeficientes.** `w_j` es cuánto cambia la predicción por **una unidad más** de `x_j`, **manteniendo las demás fijas**. Si en un modelo de precio el coeficiente de `metros` es 25,000, cada metro cuadrado adicional suma unos 25,000 pesos a igualdad de zona y antigüedad. Para comparar importancia entre variables, estandarízalas primero; y desconfía de la interpretación si las variables están muy correlacionadas entre sí (multicolinealidad), porque los coeficientes se vuelven inestables.

**Supuestos y límites.** Asume una relación lineal. Si la real es curva, el modelo tiene **sesgo alto** (underfitting): los residuos muestran un patrón en lugar de ruido. Remedios: transformar variables (`log`), agregar términos polinomiales (`x²`) o interacciones, o usar modelos no lineales ([[Árboles de decisión y random forest]], [[Gradient boosting (XGBoost y LightGBM)]]). Con muchas features, la **regularización** evita coeficientes enormes: Ridge (L2) los encoge, Lasso (L1) puede volver algunos cero.

```mermaid
flowchart LR
    X["Features x"] --> L["w·x + b"]
    L --> Y["ŷ (número)"]
    Y --> E["Error vs y real"]
    E --> O["Ajustar w, b minimizando MSE"]
    O --> L
```

### Ejemplo

```python
import numpy as np
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import make_pipeline

x = np.array([1, 2, 3]); y = np.array([2, 4, 5])
print(np.polyfit(x, y, 1).round(3))                    # [1.5   0.667]

modelo = LinearRegression().fit(x.reshape(-1, 1), y)
print(modelo.coef_.round(3), round(modelo.intercept_, 3))   # [1.5] 0.667
print(modelo.predict([[4]]).round(3))                  # [6.667]
print(round(mean_squared_error(y, modelo.predict(x.reshape(-1, 1))), 3))   # 0.056

# Relación no lineal: la recta se queda corta, el polinomio de grado 2 no
rng = np.random.default_rng(0)
X = rng.uniform(-3, 3, (200, 1))
Y = 2 * X[:, 0] ** 2 + rng.normal(0, 1, 200)
lin = LinearRegression().fit(X, Y)
poli = make_pipeline(PolynomialFeatures(2), LinearRegression()).fit(X, Y)
print("R² lineal:", round(lin.score(X, Y), 2), "| R² grado 2:", round(poli.score(X, Y), 2))
```

**En .NET.** En [[ML.NET]] el equivalente es `mlContext.Regression.Trainers.Sdca(labelColumnName: "Precio")` (o `OnlineGradientDescent`), precedido de `Concatenate("Features", ...)` y normalización. Las métricas (`MeanAbsoluteError`, `RootMeanSquaredError`, `RSquared`) se obtienen con `mlContext.Regression.Evaluate`.

### Cuándo usarlo y cuándo no
Es la **primera línea base** para cualquier problema de regresión: rápida, explicable y difícil de sobreajustar con pocas features. Úsala cuando necesitas explicar el efecto de cada variable. Pasa a modelos no lineales cuando los residuos muestran patrones o la métrica en validación se queda corta. **No** la uses para clasificar ni para predecir probabilidades: puede dar valores negativos o mayores a 1; para eso está la [[Regresión logística]].

### Errores comunes
- **Usarla para clases o probabilidades**: salidas fuera de [0, 1]; usa logística.
- **Interpretar coeficientes con variables en escalas distintas** o muy correlacionadas.
- **Extrapolar** lejos del rango de entrenamiento: la recta sigue, la realidad quizá no.
- **Ignorar outliers**: como el MSE eleva al cuadrado, un par de valores extremos jalan la recta (considera `HuberRegressor`).
- **No revisar residuos**: un patrón en ellos indica que falta no linealidad o una variable.

### Para profundizar
- [scikit-learn: modelos lineales](https://scikit-learn.org/stable/modules/linear_model.html)
- [Google ML Crash Course: regresión lineal](https://developers.google.com/machine-learning/crash-course/linear-regression)
- [Microsoft Learn: tutorial de regresión con ML.NET](https://learn.microsoft.com/es-mx/dotnet/machine-learning/tutorials/predict-prices)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Ajusta una recta a `(1,2), (2,4), (3,5)` por mínimos cuadrados.
> `x̄ = 2`, `ȳ = 11/3 ≈ 3.667`. `Sxy = 3`, `Sxx = 2` → pendiente `1.5`, intercepto `3.667 - 3 = 0.667`.
> `ŷ = 1.5x + 0.667`. Verifica: `np.polyfit([1,2,3],[2,4,5],1)` → `[1.5, 0.667]`.

> [!question]- Con esa recta, ¿qué predice para `x = 4`?
> `1.5·4 + 0.667 ≈ 6.667`

> [!question]- Calcula el MSE de esa recta sobre los 3 puntos.
> Predicciones `2.167, 3.667, 5.167`; errores `-0.167, 0.333, -0.167`.
> MSE = `(0.0278 + 0.1111 + 0.0278)/3 ≈ 0.056`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué modela la regresión lineal?
> Una salida numérica como combinación lineal de las entradas: `ŷ = w·x + b`. Se ajusta minimizando el error cuadrático medio.

> [!question]- ¿Cómo interpretas un coeficiente?
> Cuánto cambia `ŷ` por cada unidad que sube esa variable, manteniendo las demás fijas (si las variables están en escalas comparables y no muy correlacionadas).

> [!question]- ¿Qué pasa si la relación real no es lineal?
> El modelo tiene sesgo alto (underfitting). Puedes agregar features polinomiales o usar modelos no lineales (árboles, boosting).

> [!question]- Error común: usar regresión lineal para predecir una probabilidad o clase.
> Puede dar valores fuera de [0, 1]. Para clasificación usa regresión logística.

## Práctica

- [[Lab 2 - Regresión y clasificación con scikit-learn]]

