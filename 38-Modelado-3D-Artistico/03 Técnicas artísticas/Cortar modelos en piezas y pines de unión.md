---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Modelado 3D artístico
tags: [modelado-3d-artistico]
---
# Cortar modelos en piezas y pines de unión

Parte de: [[38-Modelado-3D-Artistico/03 Técnicas artísticas/_Índice Técnicas artísticas|Técnicas artísticas]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Cortar un modelo en piezas** es dividir una figura o escultura en partes que se imprimen por separado y luego se unen con **pines** (cilindros o barras que entran en agujeros de ambas piezas) y pegamento. Es una técnica básica de la impresión de figuras: permite imprimir lo que no cabe, evitar soportes difíciles, orientar cada parte para mejor calidad y pintar o imprimir en colores distintos.

### Cómo funciona
**Por qué cortar**:

1. Que quepa en la cama (256 × 256 × 256 mm en tu [[Elegoo Centauri 2 Combo]]).
2. Orientar cada parte sin soportes complicados: una cabeza boca arriba, un brazo acostado.
3. Mejor acabado: la cara visible queda hacia arriba o en paredes verticales.
4. Colores o materiales distintos por parte, o pintarlas por separado.

**Dónde cortar**: en **costuras naturales**, donde el ojo espera una línea: cuello, cintura, borde de una prenda (cinturón, cuello de camisa, puños), unión del brazo con el hombro, borde de un casco. Corta **plano**, para que cada pieza se apoye bien en la cama y para que las caras de unión coincidan.

**Pines de unión**:

```mermaid
flowchart LR
  A[Plano de corte en una costura] --> B[Boolean: dos piezas exactas]
  B --> C[Agujeros iguales en ambas piezas]
  C --> D[Pin separado impreso acostado]
  D --> E[2 pines o pin no circular]
  E --> F[Prueba en una rebanada]
  F --> G[Pegar]
```

- **Pin separado**, impreso **acostado**: sus capas corren a lo largo del pin y resiste la flexión. Un pin integrado que sale vertical se rompe por la unión entre capas; si lo integras, que sea grueso (≥ 5 mm).
- **Holgura**: de partida 0.2–0.3 mm por lado, calibrada con tu [[Prueba de tolerancias]] (ver [[Tolerancias y holguras para piezas que encajan]]).
- **Profundidad extra**: 0.5 mm más en cada agujero para que el pegamento sobrante tenga a dónde ir y las piezas cierren sin espacio.
- **Antigiro**: dos pines separados, o un pin de sección cuadrada o en "D".
- Alternativas: imanes de neodimio en huecos a presión (para piezas desmontables) o una llave en forma de cola de milano.

### Ejemplo
Pin de Ø4 mm × 10 mm con holgura de 0.25 por lado:

- Agujero: Ø4 + 2·0.25 = **Ø4.5 mm**.
- Profundidad: 5 + 0.5 = **5.5 mm** en cada pieza (11 mm en total para 10 mm de pin).

En Blender, los cortadores de los agujeros se pueden crear por script en el plano de corte:

```python
import bpy

d_pin, holgura, prof = 4.0, 0.25, 5.5
z_corte = 95.0                         # altura del plano de corte en mm
centros = [(-6.0, 0.0), (6.0, 0.0)]    # dos pines para que no gire

for i, (x, y) in enumerate(centros):
    bpy.ops.mesh.primitive_cylinder_add(
        vertices=32, radius=(d_pin + 2 * holgura) / 2, depth=2 * prof,
        location=(x, y, z_corte))
    c = bpy.context.active_object
    c.name = f"Cortador_pin_{i}"
    c.display_type = 'WIRE'
```

Cada cilindro mide 11 mm y está centrado en el plano: con Boolean *Difference* en ambas mitades, cada una recibe un agujero de 5.5 mm de profundidad.

### Práctica en tu Centauri 2
Corta tu figura en la cintura (para una figura de 180 mm, cerca de la mitad de la altura y en el borde del cinturón). Antes de imprimir las dos mitades completas, haz un Boolean *Intersect* con un cubo delgado para obtener dos rebanadas de 4 mm alrededor del corte, imprímelas con los pines y comprueba que entran sin forzar y no giran. Ajusta y luego imprime las piezas finales.

### Cuándo usarlo y cuándo no
Corta cuando ganas algo concreto: tamaño, menos soportes, mejor acabado o color. No cortes figuras pequeñas que salen bien en una pieza: cada corte es una línea que hay que rellenar, lijar y disimular al pintar (ver [[Lijado y relleno de piezas impresas]]).

### Errores comunes
- **Corte en medio de una superficie visible**: se nota siempre; usa costuras naturales.
- **Pin integrado vertical**: se rompe.
- **Un solo pin redondo**: la pieza gira.
- **Sin profundidad extra**: el pegamento no deja cerrar la unión.
- **Corte inclinado**: la pieza no se apoya en la cama.

### Para profundizar
- [Manual oficial de Blender: modificador Boolean](https://docs.blender.org/manual/en/latest/modeling/modifiers/generate/booleans.html)
- [Manual oficial de Blender: Bisect](https://docs.blender.org/manual/en/latest/modeling/meshes/editing/mesh/bisect.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Vas a usar un pin de Ø4 mm con holgura 0.25 por lado. ¿Qué agujero modelas y de qué largo si el pin mide 10 mm?
> Ø4 + 0.5 = **Ø4.5 mm**; profundidad 5 + 0.5 = **5.5 mm** en cada pieza (11 mm en total para un pin de 10).

> [!question]- Quieres que la cabeza no pueda girar sobre el cuello. Propón dos soluciones.
> 1) Dos pines separados en lugar de uno. 2) Un pin de sección cuadrada o en "D" (con su holgura).

> [!question]- Tu figura mide 180 mm de alto y la quieres en dos piezas sin que el corte se vea. ¿Dónde cortas?
> En la cintura o en el borde de una prenda (cinturón, saco), plano, cerca de la mitad de la altura para que ambas piezas sean manejables.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué cortas una figura en piezas?
> Para que quepa en la cama, para orientar cada parte sin soportes difíciles (cabeza, brazos), para imprimir partes de colores distintos o pintarlas por separado.

> [!question]- ¿Dónde cortas para que no se note?
> En **costuras naturales**: cuello, cintura, borde de ropa, unión de brazo con hombro. Corta plano para que las piezas se apoyen bien en la cama.

> [!question]- ¿Cómo diseñas pines de unión?
> Agujeros iguales en ambas piezas y un **pin separado** impreso acostado (más fuerte); holgura de partida 0.2–0.3 mm por lado (a calibrar con la [[Prueba de tolerancias]]). Usa 2 pines o un pin no circular para que no gire y agrega 0.5 mm de profundidad extra para el pegamento.

> [!question]- Error común: el pin integrado en una pieza se rompe. ¿Por qué?
> Si sale vertical, se rompe por la unión entre capas. Mejor pin separado acostado o, si es integrado, más grueso (≥ 5 mm).

## Práctica

- [[Lab 2 - Busto estilizado original esculpido con mouse]]

