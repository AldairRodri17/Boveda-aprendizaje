---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Modelado 3D técnico
herramienta: Onshape o FreeCAD + Elegoo Centauri 2 Combo con ElegooSlicer; regla, foto con el iPhone
tags: [modelado-3d-tecnico, laboratorio]
---
# Lab 5 - Reparar o adaptar una pieza rota (ingeniería inversa)

Parte de: [[37-Modelado-3D-Tecnico/04 Proyectos técnicos/_Índice Proyectos técnicos|Proyectos técnicos]]

**Cubre:** [[Repuestos y adaptadores impresos]] · [[Medir piezas e ingeniería inversa]] · [[Tolerancias y holguras para piezas que encajan]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Reproducir o adaptar una pieza de plástico rota de tu casa (perilla, clip, tapa, pata de mueble) a partir de mediciones y una foto, en máximo 3 iteraciones.

## Pasos

1. Elige una pieza **sin función de seguridad**. Junta los pedazos y identifica las medidas **críticas** (las que encajan con otra pieza) y las libres.
2. Toma una foto cenital con la regla en el mismo plano y lo más lejos posible con zoom.
3. En el CAD inserta la foto en un boceto, escálala con dos marcas de la regla y calca el contorno.
4. Redondea las medidas a valores lógicos y define las críticas como variables (`#d_eje`, `#holgura`).
5. Imprime una **pieza recortada** solo con la zona de encaje; prueba y ajusta.
6. Imprime la pieza completa en el material adecuado (PETG si hay calor o esfuerzo) y orientada para que la carga no separe capas.
7. Documenta: foto antes/después, medidas, número de iteraciones.

## Criterios de terminado

- [ ] La pieza encaja y cumple su función.
- [ ] Hice como máximo 3 impresiones de prueba recortadas antes de la final.
- [ ] El archivo CAD tiene las medidas críticas como variables.
- [ ] Escribí qué haría distinto (p. ej. comprar calibrador, otra orientación).

## Solución

> [!success]- Cómo ahorrar iteraciones
> Imprime una galga con 3 variantes de la medida crítica en una sola impresión. Si la original tenía un diámetro de 7.9 y otro de 12.1, probablemente eran 8 y 12.

> [!success]- Si se rompe donde mismo
> Esa zona concentra esfuerzo: agrega radio en la esquina interior, más material, más perímetros o cambia la orientación. Recuerda que la original quizá era de otro plástico (nylon, ABS).

## Notas personales

_Qué me costó, errores que cometí, dudas._
