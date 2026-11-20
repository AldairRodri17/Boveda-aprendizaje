---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Modelado 3D artístico
herramienta: Blender (gratuito) con mouse + Elegoo Centauri 2 Combo con ElegooSlicer
tags: [modelado-3d-artistico, laboratorio]
---
# Lab 2 - Busto estilizado original esculpido con mouse

Parte de: [[38-Modelado-3D-Artistico/01 Blender/_Índice Blender|Blender]]

**Cubre:** [[Escultura digital en Blender]] · [[Pinceles básicos de escultura en Blender]] · [[Remesh y Dyntopo en Blender]] · [[Multirresolución en Blender]] · [[Modificador Boolean en Blender]] · [[Proporciones y anatomía básica para figuras]] · [[Bustos y figuras originales]] · [[Cortar modelos en piezas y pines de unión]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Esculpir con mouse un busto estilizado **original** de unos 90 mm de alto, cortarlo en cabeza y pedestal con pines de unión e imprimirlo.

> [!warning] Propiedad intelectual
> Diseña un personaje propio. No recrees personajes con marca registrada ni de otros autores.

## Pasos

1. Dibuja en papel tu personaje de frente y perfil (3 palabras de concepto, un rasgo distintivo). Elige canon estilizado de cabeza grande.
2. Bloquea con primitivas: esfera para el cráneo, esferas o cilindros para cuello, hombros y nariz. Únelas (`Ctrl+J`).
3. Sculpt Mode: Voxel Remesh de 2 mm (`R` y `Ctrl+R`). Activa simetría X y *Stabilize Stroke*, fuerza baja.
4. Formas medias con **Clay Strips**, **Grab** para proporciones y **Smooth**. Revisa la silueta de frente, perfil y 3/4. Remesh a 0.8–1 mm.
5. Agrega **Multiresolution** (2–3 niveles) y detalla con **Crease** e **Inflate** (párpados, labios, borde del casco o cabello).
6. Aplica Multires. Crea un pedestal de cilindro con chaflán y únelo con Boolean Union.
7. Corta en el cuello con un cubo: **Difference** para el pedestal y **Intersect** para la cabeza. Resta dos cilindros de Ø4.5 × 5.5 mm en ambas caras del corte y modela 2 pines de Ø4 × 10 mm.
8. Revisa grosores (orejas, nariz) ≥ 1.2 mm y exporta las 3 piezas. Imprime con capa de 0.12–0.16 mm; cabeza con el corte en la cama.

## Criterios de terminado

- [ ] El busto es un diseño propio (tengo el boceto de referencia).
- [ ] La silueta se lee bien de frente y de perfil.
- [ ] Las dos piezas encajan con los pines y la cabeza no gira.
- [ ] Ningún detalle quedó más delgado de 1.2 mm (lo revisé con 3D Print Toolbox).
- [ ] La impresión no necesitó soportes en la cabeza o los soportes fueron mínimos.

## Solución

> [!success]- Ajustes de pincel con mouse
> Fuerza 0.3–0.5, *Stabilize Stroke* con radio 30–60 px, trazos cortos y en una sola dirección. Usa `Shift` para suavizar seguido y gira la vista a menudo.

> [!success]- Por qué cortar en el cuello
> Es una costura natural (no se nota), deja la cabeza con un corte plano para apoyarla en la cama sin soportes bajo la barbilla y te permite pintarla por separado.

> [!success]- Si los pines no entran
> Sube la holgura a 0.3 por lado o lija el pin. Si quedan flojos, un punto de pegamento de cianoacrilato.

## Notas personales

_Qué me costó, errores que cometí, dudas._
