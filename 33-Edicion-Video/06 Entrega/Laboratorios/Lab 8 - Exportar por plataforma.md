---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Edición de video
herramienta: DaVinci Resolve + HandBrake
tags: [edicion-video, laboratorio]
contenido: completo
---
# Lab 8 - Exportar por plataforma

Parte de: [[33-Edicion-Video/06 Entrega/_Índice Entrega|Entrega]]

**Cubre:** [[Exportar video (códec, bitrate y formato por plataforma)]] · [[HandBrake]] · [[Miniaturas y diseño básico]] · [[Content ID y derechos en plataformas]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Exportar el mismo proyecto en tres versiones: máster, YouTube y redes verticales, y una versión ligera para mensajería.

## Pasos

1. En Deliver, exporta un **máster** de alta calidad (ProRes 422 si tu versión lo permite en Windows, o H.265 con bitrate alto; Pendiente de confirmar disponibilidad de ProRes en Windows).
2. Exporta con el preset de **YouTube** a la resolución del proyecto.
3. Exporta la versión vertical (del Lab 3) en 1080 × 1920 H.264.
4. Con HandBrake, comprime el máster a un MP4 de menos de 100 MB para enviar por mensajería.
5. Crea una miniatura de 3840 × 2160 (lo que recomienda YouTube hoy; 1280 × 720 es el tamaño anterior y aún se acepta) con un cuadro del video y texto corto.
6. Revisa la licencia de cada canción usada y anota la fuente.

## Criterios de terminado

- [ ] Tienes 4 archivos: máster, YouTube, vertical y versión ligera (< 100 MB).
- [ ] Revisaste con las propiedades del archivo (o MediaInfo) resolución, fps y códec de cada uno, y coinciden con lo esperado.
- [ ] La miniatura mide 3840 × 2160 (16:9) y su texto se lee en tamaño pequeño.
- [ ] Hay una nota con la fuente y licencia de la música.

## Solución

> [!success]- Calcular el bitrate para la versión ligera
> Para un video de 3 min y 100 MB: 100 × 8 / 180 ≈ 4.4 Mbps totales → en HandBrake usa ~4 Mbps de video (Avg Bitrate) y 128 kbps de audio.

## Notas personales

_Qué me costó, errores que cometí, dudas._
