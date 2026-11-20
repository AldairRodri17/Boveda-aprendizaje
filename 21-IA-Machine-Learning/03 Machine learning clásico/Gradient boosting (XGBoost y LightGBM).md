---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: IA y Machine Learning
tags: [ia-machine-learning]
---
# Gradient boosting (XGBoost y LightGBM)

Parte de: [[21-IA-Machine-Learning/03 Machine learning clásico/_Índice Machine learning clásico|Machine learning clásico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El gradient boosting construye un modelo fuerte **sumando muchos árboles pequeños, uno tras otro**, donde **cada árbol nuevo corrige los errores que dejaron los anteriores**. A diferencia del random forest (árboles independientes que votan), aquí los árboles trabajan en equipo secuencial. Es el estado del arte práctico en **datos tabulares**: gana con frecuencia competencias y es el caballo de batalla en scoring de crédito, fraude, demanda y churn. **XGBoost** y **LightGBM** son las dos implementaciones más populares; scikit-learn incluye la suya (`HistGradientBoostingClassifier/Regressor`), inspirada en LightGBM.

### Cómo funciona
**Intuición con regresión.**
1. Empieza con una predicción simple: el promedio de `y`.
2. Calcula los **residuos** (lo que falta: `y − predicción`).
3. Entrena un árbol pequeño (poca profundidad) para **predecir esos residuos**.
4. Suma una fracción de ese árbol a la predicción: `pred ← pred + η·árbol`. Esa fracción `η` es la **tasa de aprendizaje** (*learning rate*, típicamente 0.05–0.1).
5. Repite cientos o miles de veces.

Se llama "gradient" porque, en general, cada árbol ajusta el **gradiente negativo de la pérdida** (para el error cuadrático coincide con el residuo; para log loss es otra expresión). Es descenso de gradiente, pero en el espacio de funciones: cada árbol es un "paso" ([[Cálculo para ML (derivadas y gradiente)]]).

```mermaid
flowchart LR
    F0["Predicción inicial (promedio)"] --> R1["Residuos"]
    R1 --> A1["Árbol 1 ajusta residuos"]
    A1 --> F1["F1 = F0 + η·árbol1"]
    F1 --> R2["Nuevos residuos"]
    R2 --> A2["Árbol 2"]
    A2 --> F2["F2 = F1 + η·árbol2"]
    F2 --> M["... hasta n árboles o early stopping"]
```

**Hiperparámetros clave** (nombres casi iguales en las tres librerías):
- `n_estimators` / número de iteraciones: cuántos árboles.
- `learning_rate`: cuánto aporta cada árbol.
- `max_depth` (XGBoost) o `num_leaves` (LightGBM): tamaño de cada árbol.
- Regularización: `min_child_weight` / `min_data_in_leaf`, `subsample`, `colsample_bytree`, penalizaciones L1/L2.

**Tasa y número de árboles están ligados.** Tasa baja + muchos árboles generaliza mejor pero tarda más. Con **tasa alta y muchísimos árboles**, el modelo sigue corrigiendo hasta **ajustar el ruido**: el error de entrenamiento sigue bajando mientras el de validación empieza a subir (sobreajuste). La defensa estándar es **early stopping**: apartas un conjunto de validación y detienes cuando la métrica deja de mejorar durante N rondas.

**XGBoost vs LightGBM vs scikit-learn.**
- **XGBoost**: muy maduro, regularización explícita, soporte de GPU, crece los árboles por nivel por defecto.
- **LightGBM**: agrupa valores en histogramas y crece los árboles **por hoja** (la hoja que más reduce la pérdida), lo que lo hace muy rápido con datasets grandes; maneja categóricas de forma nativa.
- **`HistGradientBoosting*` de scikit-learn**: sin dependencias extra, rápido, maneja nulos y tiene early stopping integrado. Excelente para empezar.

### Ejemplo
Usando la implementación de scikit-learn (la API de `xgboost.XGBClassifier` y `lightgbm.LGBMClassifier` es casi idéntica: `fit`, `predict_proba`, mismos nombres de hiperparámetros principales):

```python
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.ensemble import HistGradientBoostingClassifier, RandomForestClassifier
from sklearn.metrics import roc_auc_score

X, y = make_classification(n_samples=6_000, n_features=25, n_informative=8,
                           flip_y=0.05, random_state=0)
X_tr, X_te, y_tr, y_te = train_test_split(X, y, test_size=0.25, random_state=0)

rf = RandomForestClassifier(n_estimators=300, n_jobs=-1, random_state=0).fit(X_tr, y_tr)
gb = HistGradientBoostingClassifier(learning_rate=0.1, max_iter=1000, early_stopping=True,
                                    validation_fraction=0.15, n_iter_no_change=20,
                                    random_state=0).fit(X_tr, y_tr)
print("AUC random forest:", round(roc_auc_score(y_te, rf.predict_proba(X_te)[:, 1]), 3))
print("AUC boosting     :", round(roc_auc_score(y_te, gb.predict_proba(X_te)[:, 1]), 3))
print("árboles usados con early stopping:", gb.n_iter_, "de 1000")

# Tasa alta sin early stopping: entrenamiento perfecto, prueba peor
loco = HistGradientBoostingClassifier(learning_rate=1.0, max_iter=500, early_stopping=False,
                                      max_leaf_nodes=63, random_state=0).fit(X_tr, y_tr)
print("tasa 1.0, 500 árboles -> train", round(loco.score(X_tr, y_tr), 3), "test", round(loco.score(X_te, y_te), 3))
print("tasa 0.1 + early stop -> train", round(gb.score(X_tr, y_tr), 3), "test", round(gb.score(X_te, y_te), 3))
```

**En .NET.** [[ML.NET]] incluye `FastTree` (gradient boosting propio) y un trainer de **LightGBM** (paquete `Microsoft.ML.LightGbm`, método `mlContext.BinaryClassification.Trainers.LightGbm(...)`). También puedes entrenar XGBoost o LightGBM en Python, exportarlo a ONNX y servirlo en tu API con [[ONNX Runtime]].

### Cuándo usarlo y cuándo no
Es la mejor apuesta cuando tienes **datos tabulares** (filas y columnas de negocio) y quieres máxima precisión: suele superar a random forest con algo de ajuste. Tolera features en escalas distintas, relaciones no lineales e interacciones. Es menos adecuado para datos no estructurados (imágenes, audio, texto libre), donde las redes neuronales dominan; y si necesitas explicar cada decisión de forma simple, un modelo lineal o un árbol corto puede ser preferible (o usa SHAP para explicar el boosting).

### Errores comunes
- **Tasa alta + muchos árboles sin early stopping**: sobreajuste.
- **Ajustar todo a la vez**: empieza con tasa 0.05–0.1, early stopping y profundidad moderada; luego afina.
- **Fuga en el early stopping**: el conjunto de paro debe ser validación, no prueba.
- **Comparar contra una línea base mal hecha**: confirma primero que una logística o un bosque bien validados no bastan.

### Para profundizar
- [XGBoost: documentación oficial](https://xgboost.readthedocs.io/en/stable/)
- [LightGBM: documentación oficial](https://lightgbm.readthedocs.io/en/stable/)
- [scikit-learn: gradient boosting basado en histogramas](https://scikit-learn.org/stable/modules/ensemble.html#histogram-based-gradient-boosting)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace el gradient boosting?
> Entrena árboles pequeños en secuencia; cada uno corrige los errores (el gradiente de la pérdida) de los anteriores. La suma da un modelo fuerte.

> [!question]- ¿Cuándo es buena opción?
> En datos tabulares con relaciones no lineales; suele ser de lo más competitivo en ese tipo de datos.

> [!question]- ¿Qué pasa si subes mucho el número de árboles con una tasa de aprendizaje alta?
> Sobreajusta. Se combina tasa baja + *early stopping* sobre un conjunto de validación.

