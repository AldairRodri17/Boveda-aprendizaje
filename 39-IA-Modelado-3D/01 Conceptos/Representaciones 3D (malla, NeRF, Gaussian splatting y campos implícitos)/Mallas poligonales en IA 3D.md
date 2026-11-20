---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: IA para modelado 3D
tags: [ia-modelado-3d]
---
# Mallas poligonales en IA 3D

Parte de: [[Representaciones 3D (malla, NeRF, Gaussian splatting y campos implícitos)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **malla poligonal** es un conjunto de **vértices** (puntos con coordenadas X, Y, Z), **aristas** que los unen y **caras** (casi siempre triángulos) que forman la superficie del objeto. Es la representación final que entregan los modelos de imagen a 3D en archivos OBJ o GLB, y la única que entienden los laminadores (STL, 3MF, OBJ). Todo lo que hace la IA termina, tarde o temprano, convertido en una malla.

### Cómo funciona
Para que una malla se pueda imprimir debe describir un **sólido**, no solo una piel:
- **Manifold (cerrada, "hermética"):** cada arista la comparten exactamente dos caras. Si hay bordes sueltos, el laminador no sabe qué está dentro y qué fuera.
- **Normales hacia afuera:** cada triángulo tiene un frente; si algunos apuntan hacia adentro, el laminador puede interpretar huecos donde no los hay.
- **Sin auto-intersecciones:** partes de la superficie que se atraviesan entre sí generan capas raras.
- **Grosor suficiente:** una pared de 0.3 mm existe en la malla pero no la puede trazar una boquilla de 0.4 mm.

¿Por qué las mallas de IA se ven tan desordenadas? Porque salen de **marching cubes** sobre una rejilla: triángulos de tamaño parecido repartidos por toda la superficie, sin "bucles" de aristas que sigan la forma. A un modelador humano le estorba esa topología para animar o editar, pero **para imprimir da igual**: el laminador solo corta el sólido en capas. Si quisieras editarla con comodidad o animarla, harías [[Retopología básica]].

### Ejemplo
TripoSR te entrega `0/mesh.obj` con unos 200,000 triángulos y una altura de 1.62 unidades. En Blender:
```text
1. File > Import > Wavefront (.obj)
2. Panel N > Dimensions: 1.62 de alto -> escala x55.56 para 90 mm; Ctrl+A > Scale
3. Edit Mode > Select All > Mesh > Clean Up > Merge by Distance
4. Select > Select All by Trait > Non Manifold  (¿hay bordes abiertos?)
5. Modificador Remesh (Voxel, 0.4 mm) para cerrar todo en un solo cuerpo
6. Modificador Decimate (Collapse) hasta ~150k triángulos si quedó pesada
7. File > Export > STL
```
Antes y después del Remesh, mira el contador de triángulos en la barra de estado (actívalo en *Scene Statistics*). Para una figura de 90 mm impresa con boquilla de 0.4 mm y capas de 0.12–0.2 mm, entre 100,000 y 300,000 triángulos sobra; millones no aportan nada visible.

### Cuándo preocuparte por la malla
- **Siempre** revisa manifold y grosores antes de laminar ([[Reparar mallas para impresión]]).
- **Decimar** cuando Blender o ElegooSlicer se ponen lentos, o el STL pesa cientos de MB.
- **Retopologizar** solo si vas a animar, hacer UV limpias o esculpir con control fino; para imprimir no hace falta.

### Errores comunes
1. **Imprimir sin revisar el conteo de triángulos.** Una malla de varios millones hace lento todo el flujo y el detalle extra se pierde en la boquilla. Usa *Decimate* con un ratio que conserve la silueta.
2. **Asumir que "se ve cerrada" es cerrada.** Huecos de un solo triángulo no se notan a simple vista; usa *Select Non Manifold* o el [3D Print Toolbox](https://extensions.blender.org/add-ons/print3d-toolbox/).
3. **Separar colores con `P` y dejar piezas abiertas.** Cada pieza debe quedar cerrada; ver [[Separar un modelo por colores para CANVAS]].
4. **Olvidar aplicar la escala.** El STL se exporta con la escala aplicada o no según el exportador; aplícala siempre.

### Para profundizar
- [Modificador Decimate (manual de Blender)](https://www.blender.org/manual/en/modeling/modifiers/generate/decimate.html)
- [Modificador Remesh (manual de Blender)](https://www.blender.org/manual/en/modeling/modifiers/generate/remesh.html)
- [Importar y exportar STL (manual de Blender 4.2)](https://docs.blender.org/manual/en/4.2/files/import_export/stl.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una malla y qué tiene que cumplir para imprimirse?
> Un conjunto de vértices, aristas y caras (triángulos). Para imprimir debe ser **manifold** (cerrada, sin bordes sueltos), con normales hacia afuera y sin auto-intersecciones. Ver [[Reparar mallas para impresión]].

> [!question]- ¿Por qué las mallas de IA tienen tantos triángulos desordenados?
> Porque salen de marching cubes: triángulos de tamaño parejo en toda la superficie, sin bucles de aristas. Sirven para imprimir; para animar o editar se haría [[Retopología básica]].

> [!question]- Error común: imprimir sin revisar el conteo de triángulos. ¿Qué puede pasar?
> Una malla de millones de triángulos hace lento a Blender y al laminador, y no da más detalle que el que la boquilla de 0.4 mm puede reproducir. Reduce con *Decimate* antes de exportar.

