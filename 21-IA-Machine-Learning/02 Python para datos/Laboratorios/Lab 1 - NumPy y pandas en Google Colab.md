---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: IA y Machine Learning
herramienta: Google Colab (o Jupyter local) con NumPy, pandas y seaborn
tags: [ia-machine-learning, laboratorio]
---
# Lab 1 - NumPy y pandas en Google Colab

Parte de: [[21-IA-Machine-Learning/02 Python para datos/_Índice Python para datos|Python para datos]]

**Cubre:** [[Jupyter notebooks]] · [[NumPy]] · [[pandas]] · [[Visualización con matplotlib y seaborn]] · [[Estadística para ML (distribuciones, media, varianza)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Practicar operaciones vectorizadas con NumPy y un análisis exploratorio completo con pandas y seaborn sobre un dataset público (el de propinas `tips` que trae seaborn).

## Pasos

1. Abre un notebook nuevo en Google Colab (o en Jupyter local con un entorno virtual).
2. Con NumPy crea `a = np.arange(12).reshape(3, 4)` y calcula: suma por columnas, media por filas, los elementos pares y la versión normalizada por columna (media 0, desviación 1).
3. Calcula media, mediana, varianza poblacional y muestral de `[2, 4, 4, 4, 5, 5, 7, 9]` y compara con tus cálculos a mano.
4. Carga el dataset con `sns.load_dataset('tips')`. Revisa `head()`, `info()`, `describe()` y nulos.
5. Crea la columna `pct_propina = tip / total_bill`.
6. Responde con `groupby`: propina porcentual promedio por día y por fumador/no fumador.
7. Haz tres gráficas: histograma de `total_bill`, boxplot de `pct_propina` por `day` y dispersión `total_bill` vs `tip` coloreada por `time`.
8. Escribe en una celda de texto tres conclusiones. Ejecuta *Restart & Run All* para confirmar que todo corre en orden.

## Criterios de terminado

- [ ] `a.sum(axis=0)` devuelve `[12, 15, 18, 21]` y las columnas normalizadas tienen media ≈ 0 y desviación ≈ 1.
- [ ] La varianza muestral calculada da ≈ 4.571 y la poblacional 4.0.
- [ ] Existe la columna `pct_propina` y la tabla de promedios por día tiene 4 filas.
- [ ] Las tres gráficas tienen título y ejes con nombre.
- [ ] El notebook corre completo con *Restart & Run All* sin errores.

## Solución

> [!success]- NumPy
> ```python
> import numpy as np
> a = np.arange(12).reshape(3, 4)
> a.sum(axis=0)          # [12 15 18 21]
> a.mean(axis=1)         # [1.5 5.5 9.5]
> a[a % 2 == 0]          # [ 0  2  4  6  8 10]
> z = (a - a.mean(axis=0)) / a.std(axis=0)
> z.mean(axis=0), z.std(axis=0)
> d = np.array([2,4,4,4,5,5,7,9])
> d.mean(), np.median(d), d.var(), d.var(ddof=1)
> ```

> [!success]- pandas y seaborn
> ```python
> import seaborn as sns, matplotlib.pyplot as plt
> tips = sns.load_dataset('tips')
> tips.info(); tips.isna().sum()
> tips['pct_propina'] = tips['tip'] / tips['total_bill']
> tips.groupby('day', observed=True)['pct_propina'].mean()
> tips.groupby('smoker', observed=True)['pct_propina'].mean()
>
> sns.histplot(tips['total_bill']); plt.title('Cuenta total'); plt.show()
> sns.boxplot(data=tips, x='day', y='pct_propina'); plt.title('% propina por día'); plt.show()
> sns.scatterplot(data=tips, x='total_bill', y='tip', hue='time'); plt.title('Cuenta vs propina'); plt.show()
> ```
> `observed=True` evita avisos porque `day` es categórica.

## Notas personales

_Qué me costó, errores que cometí, dudas._
