---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Edición de video
herramienta: DaVinci Resolve (Gyroflow si tu cámara es compatible)
tags: [edicion-video, laboratorio]
contenido: completo
---
# Lab 7 - Rampa de velocidad y estabilización

Parte de: [[33-Edicion-Video/05 Gráficos y efectos/_Índice Gráficos y efectos|Gráficos y efectos]]

**Cubre:** [[Rampas de velocidad]] · [[Estabilización en posproducción (Gyroflow)]] · [[Transiciones con criterio]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear una rampa de velocidad con material de 60 o 120 fps y estabilizar una toma en mano.

## Pasos

1. Importa un clip a 60 o 120 fps del Lab 1 de Video y audio en una línea de tiempo de 30 fps.
2. Abre Retime Controls (Ctrl+R) y crea una rampa: 100 % → 25 % → 100 %.
3. Suaviza la transición con la curva de retime.
4. Toma un clip en mano tembloroso y aplica la estabilización del Inspector; compara antes/después.
5. Si tienes una cámara compatible con Gyroflow, estabiliza el mismo tipo de toma ahí y compara.

## Criterios de terminado

- [ ] La rampa no muestra cuadros repetidos en la parte lenta.
- [ ] El cambio de velocidad es gradual (no salta).
- [ ] Tienes una comparación antes/después de la estabilización y anotaste cuánto recorte se perdió.

## Solución

> [!success]- Qué velocidad mínima aguanta cada clip sin interpolar
> En un proyecto de 30 fps: clip de 60 fps → 50 %; 120 fps → 25 %; 240 fps → 12.5 %.

## Notas personales

_Qué me costó, errores que cometí, dudas._
