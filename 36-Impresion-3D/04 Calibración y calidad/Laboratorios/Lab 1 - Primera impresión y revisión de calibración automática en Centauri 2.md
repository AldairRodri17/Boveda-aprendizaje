---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Impresión 3D
herramienta: Centauri 2 Combo + ElegooSlicer/OrcaSlicer
tags: [impresion-3d, laboratorio]
---
# Lab 1 - Primera impresión y revisión de calibración automática en Centauri 2

Parte de: [[36-Impresion-3D/04 Calibración y calidad/_Índice Calibración y calidad|Calibración y calidad]]

**Cubre:** [[Elegoo Centauri 2 Combo]] · [[Primera capa y calibración automática]] · [[Seguridad al imprimir (ventilación, emisiones y ubicación)]] · [[ElegooSlicer y OrcaSlicer]] · [[PLA]] · [[Adherencia a la cama (brim, raft y superficies)]] · [[Herramientas básicas para impresión 3D]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Armar la impresora de forma segura, completar su calibración automática y obtener una primera impresión de PLA con una primera capa correcta.

## Pasos

1. Coloca la impresora en una superficie firme, en un cuarto ventilado y lejos de cortinas o papeles; deja espacio alrededor para que el cabezal y la cama se muevan.
2. Retira todos los seguros de transporte según la guía rápida y revisa el inventario de la caja.
3. Enciende, completa el asistente inicial y ejecuta la calibración automática (nivelación y Z offset). Anota qué pasos hace y cuánto tarda.
4. Instala ElegooSlicer, agrega la impresora *Centauri 2* (con CANVAS) y conéctala por Wi‑Fi si lo deseas.
5. Lava la placa con agua y jabón, sécala y colócala sin tocar la superficie.
6. Carga PLA en el CANVAS e imprime un modelo de prueba incluido o un cuadro de primera capa de 1 capa y 100 × 100 mm.
7. Observa la primera capa completa sin alejarte; toma foto con el iPhone de cerca.
8. Imprime después una pieza pequeña (por ejemplo, un *benchy* o un llavero) y quédate supervisando al menos los primeros 15 minutos.

## Criterios de terminado

- [ ] La calibración automática terminó sin errores y anoté sus pasos.
- [ ] La primera capa tiene líneas unidas, sin huecos ni crestas (foto guardada).
- [ ] La pieza de prueba se despegó doblando la placa ya fría y no tiene esquinas levantadas.
- [ ] Anoté en mi nota: temperatura usada, tiempo estimado vs real y gramos que reportó el laminador.

## Solución

> [!success]- Si las líneas quedan redondas y separadas
> La boquilla está alta: repite la calibración o baja el Z offset en pasos pequeños durante la primera capa. Revisa también grasa en la placa.

> [!success]- Si la boquilla raspa o hay líneas transparentes
> Está demasiado baja: sube ligeramente el Z offset. Verifica que no haya basura debajo de la placa flexible.

> [!success]- Tiempo real mayor que el estimado
> Es normal: el estimado no siempre incluye calentamiento y calibración previa.

## Notas personales

_Qué me costó, errores que cometí, dudas._
