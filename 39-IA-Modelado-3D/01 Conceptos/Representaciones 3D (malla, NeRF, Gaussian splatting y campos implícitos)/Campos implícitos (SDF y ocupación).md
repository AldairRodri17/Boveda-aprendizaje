---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: IA para modelado 3D
tags: [ia-modelado-3d]
---
# Campos implícitos (SDF y ocupación)

Parte de: [[Representaciones 3D (malla, NeRF, Gaussian splatting y campos implícitos)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **campo implícito** describe un objeto con una función que puedes evaluar en cualquier punto del espacio, en lugar de una lista de triángulos. Hay dos variantes principales: el **SDF** (*signed distance function*, función de distancia con signo), que da la distancia a la superficie, y el **campo de ocupación**, que da la probabilidad de que el punto esté dentro del objeto. Muchos generadores modernos (por ejemplo los VAE de forma de Hunyuan3D) aprenden algo de este tipo porque es fácil de manejar para una red neuronal.

### Cómo funciona
**SDF.** Para cada punto `p`, `f(p)` es la distancia a la superficie más cercana: **negativa adentro, positiva afuera y cero justo en la superficie**. Una esfera de radio 10 mm centrada en el origen es `f(p) = |p| − 10`: en el centro vale −10, a 15 mm vale +5, y en la superficie vale 0.

**Ocupación.** Para cada punto, `o(p)` va de 0 a 1: cerca de 1 si está dentro, cerca de 0 si está fuera. La superficie se toma donde cruza un umbral, típicamente 0.5.

**De campo a malla.**
1. Se evalúa la función en una rejilla 3D.
2. **Marching cubes** busca dónde cambia el signo (SDF) o dónde se cruza el umbral (ocupación) y coloca triángulos ahí.
3. Algunos modelos usan variantes (FlexiCubes, octrees) para dar más detalle con menos memoria; en Hunyuan3D el parámetro `octree_resolution` controla esa finura.

```python
import numpy as np
from skimage import measure  # pip install scikit-image

# SDF de una esfera de radio 10 en una rejilla de 64^3 (unidades = mm)
n = 64
x = np.linspace(-15, 15, n)
X, Y, Z = np.meshgrid(x, x, x, indexing="ij")
sdf = np.sqrt(X**2 + Y**2 + Z**2) - 10.0

# Extrae la isosuperficie donde la distancia es 0
verts, faces, _, _ = measure.marching_cubes(sdf, level=0.0, spacing=(x[1] - x[0],) * 3)
print(len(verts), "vértices,", len(faces), "triángulos")
```
Con `n = 128` obtendrías una esfera más redonda y unas cuatro veces más triángulos: el mismo intercambio detalle/memoria que ves en los modelos de IA.

### Por qué dan mallas cerradas
Un campo implícito **divide todo el espacio en dentro y fuera**. La frontera entre ambas regiones, por definición, encierra un volumen: no puede quedar un borde suelto en medio de la nada. Por eso las mallas que salen de estos modelos suelen ser **herméticas** de origen, ideales para imprimir. Lo que sí puede aparecer:
- **Islas flotantes:** pequeñas regiones "dentro" separadas del cuerpo principal (ruido del modelo).
- **Burbujas internas:** huecos cerrados dentro de la pieza.
- **Escalones:** artefactos de la rejilla si la resolución es baja.

Todo eso se limpia en Blender borrando piezas sueltas y aplicando Voxel Remesh ([[Limpiar un modelo generado por IA en Blender]]).

### Cuándo te sirve saberlo
- Para entender por qué subir la resolución de la rejilla da más detalle y consume más VRAM ([[VRAM - medirla y qué hacer cuando no alcanza]]).
- Para interpretar parámetros como `octree_resolution` o `mc_resolution` en las interfaces Gradio.
- Para saber que **Voxel Remesh** de Blender hace algo parecido: convierte tu malla en un volumen y vuelve a extraer la superficie, cerrándola.

### Errores comunes
1. **Pensar que "cerrada" significa "lista".** Puede estar cerrada y tener islas, burbujas o paredes de 0.5 mm.
2. **Usar una rejilla muy gruesa.** Detalles más pequeños que una celda desaparecen (dedos, orejas finas).
3. **Confundir el signo.** En un SDF negativo es adentro; si una herramienta usa la convención contraria, la malla sale con normales invertidas. Recalcúlalas en Blender (`Shift+N`).

### Para profundizar
- [Signed distance function (Wikipedia)](https://en.wikipedia.org/wiki/Signed_distance_function)
- [Marching cubes (Wikipedia)](https://en.wikipedia.org/wiki/Marching_cubes)
- [Hunyuan3D-2 en GitHub](https://github.com/Tencent/Hunyuan3D-2)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un SDF?
> Una *signed distance function*: para cada punto da la distancia a la superficie, negativa adentro y positiva afuera. La superficie es donde vale 0.

> [!question]- ¿Qué es un campo de ocupación?
> Una función que dice la probabilidad de que un punto esté **dentro** del objeto. La superficie se toma donde cruza un umbral (p. ej. 0.5). Los VAE de forma de Hunyuan3D trabajan con representaciones de este tipo.

> [!question]- ¿Por qué los campos implícitos tienden a dar mallas cerradas?
> Porque separan el espacio en dentro y fuera; al extraer la isosuperficie sale una superficie que encierra un volumen, ideal para imprimir (aunque puede traer piezas flotantes pequeñas).

