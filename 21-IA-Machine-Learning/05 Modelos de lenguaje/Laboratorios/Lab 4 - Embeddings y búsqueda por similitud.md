---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: IA y Machine Learning
herramienta: Google Colab con sentence-transformers y NumPy
tags: [ia-machine-learning, laboratorio]
contenido: completo
---
# Lab 4 - Embeddings y búsqueda por similitud

Parte de: [[21-IA-Machine-Learning/05 Modelos de lenguaje/_Índice Modelos de lenguaje|Modelos de lenguaje]]

**Cubre:** [[Embeddings]] · [[Embeddings y bases vectoriales (pgvector y Azure AI Search)]] · [[Álgebra lineal para ML (vectores, matrices y producto punto)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Generar embeddings de frases con un modelo abierto, buscar por similitud coseno con NumPy y comparar contra búsqueda por palabras clave.

## Pasos

1. En Colab instala `sentence-transformers` (`pip install sentence-transformers`).
2. Escribe una lista de 15–20 frases cortas en español sobre 3 temas distintos (p. ej. pagos, redes, cocina).
3. Carga un modelo multilingüe pequeño, por ejemplo `paraphrase-multilingual-MiniLM-L12-v2` (si no está disponible, elige otro multilingüe de sentence-transformers) y genera los embeddings normalizados.
4. Revisa la forma de la matriz de embeddings (`(n, d)`).
5. Implementa `buscar(consulta, k=3)` con producto punto (coseno, porque están normalizados) y `np.argsort`.
6. Prueba 3 consultas con palabras que **no** aparecen literalmente en las frases (sinónimos).
7. Compara con una búsqueda ingenua por palabras compartidas y anota en qué casos gana cada una.
8. Opcional: agrupa los embeddings con k-means (k = 3) y revisa si los grupos coinciden con tus temas.

## Criterios de terminado

- [ ] La matriz de embeddings tiene una fila por frase y la norma de cada fila es ≈ 1.
- [ ] `buscar` devuelve las k frases con su puntaje en orden descendente.
- [ ] Al menos 2 de las 3 consultas con sinónimos devuelven una frase del tema correcto en el primer lugar.
- [ ] Hay una nota escrita comparando búsqueda semántica vs por palabras.

## Solución

> [!success]- Código
> ```python
> !pip -q install sentence-transformers
> import numpy as np
> from sentence_transformers import SentenceTransformer
>
> frases = [
>   'El cliente pagó con tarjeta de crédito', 'El cargo fue rechazado por el banco',
>   'Generamos una referencia para pagar en OXXO', 'La transferencia SPEI llegó en minutos',
>   'El router asigna direcciones con DHCP', 'La máscara /24 tiene 254 hosts útiles',
>   'El switch aprende direcciones MAC', 'Configuramos una VLAN para voz',
>   'Hierve el agua y agrega la pasta', 'Sofríe la cebolla con ajo',
>   'Hornea el pastel a 180 grados', 'Agrega sal al gusto',
> ]
> modelo = SentenceTransformer('paraphrase-multilingual-MiniLM-L12-v2')
> E = modelo.encode(frases, normalize_embeddings=True)
> print(E.shape, np.linalg.norm(E, axis=1)[:3])
>
> def buscar(consulta, k=3):
>     q = modelo.encode([consulta], normalize_embeddings=True)[0]
>     s = E @ q
>     idx = np.argsort(-s)[:k]
>     return [(frases[i], round(float(s[i]), 3)) for i in idx]
>
> for c in ['abono bancario', 'receta de comida', 'red local de la oficina']:
>     print(c, buscar(c))
> ```

> [!success]- Búsqueda por palabras para comparar
> ```python
> def por_palabras(consulta, k=3):
>     q = set(consulta.lower().split())
>     s = [len(q & set(f.lower().split())) for f in frases]
>     return [frases[i] for i in np.argsort(s)[::-1][:k]]
> ```
> Con sinónimos ("abono" vs "transferencia") la búsqueda por palabras suele devolver resultados al azar (puntaje 0), mientras que la semántica encuentra el tema. Con códigos exactos o siglas raras, la de palabras puede ganar: por eso existe la búsqueda híbrida.

## Notas personales

_Qué me costó, errores que cometí, dudas._
