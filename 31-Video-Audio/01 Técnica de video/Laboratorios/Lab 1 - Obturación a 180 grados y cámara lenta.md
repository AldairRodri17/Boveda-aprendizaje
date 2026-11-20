---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Video y audio
herramienta: iPhone 15 Pro + Blackmagic Camera
tags: [video-audio, laboratorio]
---
# Lab 1 - Obturación a 180 grados y cámara lenta

Parte de: [[31-Video-Audio/01 Técnica de video/_Índice Técnica de video|Técnica de video]]

**Cubre:** [[Resolución y cuadros por segundo]] · [[Regla de los 180 grados del obturador]] · [[Cámara lenta (high frame rate)]] · [[Bitrate y tamaño de archivo]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Ver con tus propios ojos cómo la velocidad de obturación cambia el desenfoque de movimiento y cómo se consigue una cámara lenta limpia, usando solo el iPhone.

## Pasos

1. En [[Blackmagic Camera (app)]] elige 4K a 30 fps, ISO fijo y balance de blancos fijo (no automáticos).
2. Pide a alguien que agite la mano o camine frente a la cámara. Graba 10 s con obturación a **180° (1/60)**.
3. Repite la misma acción con **1/500** o más rápido (si la imagen se oscurece, sube el ISO).
4. Repite con **360° (1/30)**.
5. Cambia a 1080p a **120 fps** y obturación **1/240**; graba la misma acción. Si la app Cámara de Apple te da más opciones (240 fps), haz también una toma en su modo Cámara lenta.
6. Anota en una tabla el tamaño de cada archivo y su duración; calcula el bitrate aproximado.
7. Mira los clips en la computadora, cuadro por cuadro.

## Criterios de terminado

- [ ] Tienes 4 clips de la misma acción con 1/30, 1/60, 1/500 y 1/240 a 120 fps, y sabes cuál es cuál por el nombre del archivo o tu tabla.
- [ ] Describes en una frase la diferencia de desenfoque entre 1/60 y 1/500.
- [ ] El clip de 120 fps, puesto en una línea de tiempo de 30 fps, dura 4 veces más que la acción real.
- [ ] Calculaste el bitrate de al menos 2 clips (Mbps = MB × 8 / s).

## Solución

> [!success]- Qué deberías observar
> - **1/60 (180°)**: la mano tiene una estela ligera; el movimiento se ve natural.
> - **1/500**: cada cuadro nítido; el movimiento se ve a saltos.
> - **1/30 (360°)**: estelas largas, aspecto "soñado" o borroso.
> - **120 fps a 1/240**: movimiento suave al ralentizarlo; con poca luz notarás más ruido.

> [!success]- Cálculo de bitrate
> Ejemplo: un clip de 10 s que pesa 95 MB → 95 × 8 / 10 = **76 Mbps**.
> Un clip de 120 fps a 30 fps: 10 s reales × 120 = 1200 cuadros / 30 = **40 s** (25 %).

## Notas personales

_Qué me costó, errores que cometí, dudas._
