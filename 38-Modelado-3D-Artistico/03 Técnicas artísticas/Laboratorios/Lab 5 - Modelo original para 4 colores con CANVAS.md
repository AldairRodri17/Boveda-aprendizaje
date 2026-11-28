---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Modelado 3D artístico
herramienta: Blender (gratuito) + extensión 3MF Import/Export + Elegoo Centauri 2 Combo con CANVAS y ElegooSlicer
tags: [modelado-3d-artistico, laboratorio]
contenido: completo
---
# Lab 5 - Modelo original para 4 colores con CANVAS

Parte de: [[38-Modelado-3D-Artistico/03 Técnicas artísticas/_Índice Técnicas artísticas|Técnicas artísticas]]

**Cubre:** [[Modelos pensados para impresión multicolor]] · [[Preparar un modelo de Blender para imprimir]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Diseñar un modelo **original** (insignia, llavero o figura pequeña) pensado para 4 colores del CANVAS, minimizando cambios de filamento.

## Pasos

1. Dibuja tu diseño con exactamente 4 colores y decide cuál es la base (el que más área ocupa).
2. En Blender modela la base (p. ej. 60 × 60 × 3 mm) y cada color de detalle como objeto separado, incrustado o en relieve de 0.6 mm en la cara superior.
3. Asegura que ningún detalle de color mida menos de 1 mm de ancho.
4. Opción A: asigna un material de color a cada objeto y exporta 3MF. Opción B: exporta un STL por color.
5. En ElegooSlicer carga el 3MF (o los STL como un solo objeto con varias partes) y asigna los 4 filamentos ([[Impresión multicolor (pintar colores y torre de purga)]]).
6. Revisa en la vista previa en cuántas capas hay cambios de color y el desperdicio de purga estimado.
7. Haz una variante en que todo el color esté en las 3 últimas capas y compara tiempo y desperdicio. Imprime la mejor.

## Criterios de terminado

- [ ] El diseño es original y usa 4 colores.
- [ ] Los colores quedaron asignados por pieza sin pintar a mano en el laminador.
- [ ] Comparé dos variantes y anoté tiempo y purga de cada una.
- [ ] La impresión final tiene bordes de color limpios.

## Solución

> [!success]- Por qué concentrar los colores arriba
> El laminador solo cambia filamento en las capas donde hay más de un color. Si el color está en las últimas 3 capas, purgas pocas veces; si está en paredes laterales de toda la altura, purgas en cada capa.

> [!success]- Si el laminador no reconoce los colores del 3MF
> Carga los STL por separado como partes de un mismo objeto y asigna el filamento en la lista de objetos; o pinta las caras con la herramienta de pintura multicolor.

## Notas personales

_Qué me costó, errores que cometí, dudas._
