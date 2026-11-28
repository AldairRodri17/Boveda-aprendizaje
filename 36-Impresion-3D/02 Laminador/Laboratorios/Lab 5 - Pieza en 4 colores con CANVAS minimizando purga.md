---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Impresión 3D
herramienta: Centauri 2 Combo + ElegooSlicer/OrcaSlicer
tags: [impresion-3d, laboratorio]
contenido: completo
---
# Lab 5 - Pieza en 4 colores con CANVAS minimizando purga

Parte de: [[36-Impresion-3D/02 Laminador/_Índice Laminador|Laminador]]

**Cubre:** [[Impresión multicolor (pintar colores y torre de purga)]] · [[Sistema multicolor CANVAS de Elegoo]] · [[Archivos para imprimir (STL, 3MF, OBJ y STEP)]] · [[Costeo de una impresión 3D]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Imprimir una pieza de 4 colores con el CANVAS y reducir de forma medible el filamento desperdiciado en purga.

## Pasos

1. Carga 4 carretes de PLA en el CANVAS (claros y oscuros) y verifica que la impresora reconozca cada ranura.
2. Elige un modelo simple (letrero, llavero o placa con logo) y ábrelo en ElegooSlicer.
3. Versión A: pinta colores libremente en varias alturas. Lamina y anota gramos de pieza, gramos de purga, número de cambios y tiempo.
4. Versión B: rediseña la asignación para que cada color ocupe un rango de capas (por ejemplo, base de un color y relieve de los otros tres solo en las últimas capas). Activa purgar en relleno u objeto de sacrificio si está disponible y revisa los volúmenes de purga (claro→oscuro menos, oscuro→claro más).
5. Lamina la versión B y anota los mismos datos.
6. Imprime la versión B y revisa contaminación de color.

## Criterios de terminado

- [ ] Tengo una tabla A vs B con cambios, gramos de purga y tiempo.
- [ ] La versión B tiene al menos 50 % menos purga que la A.
- [ ] La pieza impresa no muestra mezcla de colores visible a 30 cm.
- [ ] Guardé el proyecto como 3MF.

## Solución

> [!success]- Por qué funciona
> La purga es casi proporcional al número de cambios. Agrupar cada color en rangos de capas reduce los cambios de cientos a decenas. Ver ejercicios de [[Impresión multicolor (pintar colores y torre de purga)]].

> [!success]- Si hay contaminación
> Aumenta la purga solo en las transiciones oscuro→claro en vez de subirla en todas.

## Notas personales

_Qué me costó, errores que cometí, dudas._
