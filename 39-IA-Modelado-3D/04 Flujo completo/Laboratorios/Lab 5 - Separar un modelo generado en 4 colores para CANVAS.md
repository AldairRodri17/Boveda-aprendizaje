---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: IA para modelado 3D
herramienta: Blender (Boolean, extensión 3MF Import/Export), ElegooSlicer, Elegoo Centauri 2 Combo con CANVAS
tags: [ia-modelado-3d, laboratorio]
contenido: completo
---
# Lab 5 - Separar un modelo generado en 4 colores para CANVAS

Parte de: [[39-IA-Modelado-3D/04 Flujo completo/_Índice Flujo completo|Flujo completo]]

**Cubre:** [[Separar un modelo por colores para CANVAS]] · [[Limpiar un modelo generado por IA en Blender]] · [[De la IA a la impresora (flujo completo)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Convertir un modelo generado (por ejemplo, la figura del Lab 4) en una impresión de hasta 4 colores con el CANVAS, comparando pintar en el laminador contra partes separadas en Blender.

## Pasos

1. Usa el modelo limpio del Lab 4 y conserva una copia con los colores originales de la IA (GLB) como referencia.
2. Decide 4 colores: uno base y tres de detalle en zonas compactas. Dibuja en papel dónde va cada uno.
3. **Método A (pintar)**: importa el STL en ElegooSlicer, asigna los 4 filamentos a las ranuras del CANVAS y pinta las caras con la herramienta de color. Lamina y anota tiempo, filamento y tamaño de la torre de purga.
4. **Método B (partes)**: en Blender, crea objetos cortadores para cada zona de color y usa Boolean *Intersect* para obtener la pieza de color y *Difference* para restarla al cuerpo base. Cada pieza debe quedar cerrada.
5. Exporta todas las piezas en un 3MF con la extensión [3MF Import/Export](https://extensions.blender.org/add-ons/threemf-io/) (o un STL por color), cárgalas como un objeto con varias partes y asigna un filamento a cada parte.
6. Compara ambos métodos (tiempo, filamento, calidad de bordes) e imprime el mejor.

## Criterios de terminado

- [ ] Tengo una tabla A vs B con tiempo estimado, filamento total y desperdicio/purga.
- [ ] En el método B cada pieza pasa *Solid* en 3D Print Toolbox.
- [ ] La impresión tiene los 4 colores en su lugar, sin mezclas visibles en los bordes.
- [ ] Anoté qué cambiaría en el diseño para reducir cambios de color.

## Solución

> [!success]- Cuál método conviene
> Pintar en el laminador es más rápido de preparar y basta para zonas grandes. Las partes en Blender dan bordes más limpios y repetibles, y permiten detalles en relieve. En ambos casos, entre menos capas con cambios de color, menos purga.

> [!success]- Si el laminador rechaza las partes
> Alguna pieza quedó abierta: probablemente usaste *Separate* (`P`) en vez de Boolean. Repite el corte con Boolean o aplica Voxel Remesh a la pieza (y verifica que siga encajando).

> [!success]- Si los detalles de color se pierden
> Cada zona de color debe medir al menos 2 anchos de línea (~0.8–1 mm). Agranda la zona o simplifica. Ver [[Modelos pensados para impresión multicolor]].

## Notas personales

_Qué me costó, errores que cometí, dudas._
