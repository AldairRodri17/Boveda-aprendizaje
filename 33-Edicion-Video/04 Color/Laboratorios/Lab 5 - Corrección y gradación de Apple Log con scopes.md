---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Edición de video
herramienta: DaVinci Resolve
tags: [edicion-video, laboratorio]
contenido: completo
---
# Lab 5 - Corrección y gradación de Apple Log con scopes

Parte de: [[33-Edicion-Video/04 Color/_Índice Color|Color]]

**Cubre:** [[Corrección de color vs gradación]] · [[Scopes (waveform, vectorscopio y parade)]] · [[Waveform en corrección de color]] · [[Vectorscopio]] · [[Parade RGB]] · [[Material Log y LUTs]] · [[Nodos de color en DaVinci Resolve]] · [[Igualar tomas y tonos de piel]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Convertir un clip Apple Log a Rec.709, corregirlo con scopes, igualarlo con otra toma y darle un look.

## Pasos

1. Importa el clip Apple Log y el estándar del Lab 2 de Video y audio (u otros dos clips de la misma escena).
2. En Color, crea 4 nodos en serie (Alt+S) y nómbralos: Balance, Contraste, Conversión, Look.
3. En **Conversión** aplica la LUT oficial de Apple Log a Rec.709 (descárgala de Apple y cópiala en la carpeta de LUTs de Resolve) o usa Color Space Transform con entrada Apple Log.
4. En **Balance**, ajusta exposición viendo el waveform y el balance de blancos con el parade sobre un área neutra.
5. En **Contraste**, ajusta contraste y saturación; revisa la piel en el vectorscopio.
6. Guarda un still del clip 1 e iguala el clip 2 con pantalla dividida.
7. En **Look**, aplica una gradación sutil (p. ej. sombras frías y luces cálidas).

## Criterios de terminado

- [ ] El clip Log ya no se ve gris: el waveform ocupa el rango de negros a altas luces sin recorte involuntario.
- [ ] En un objeto neutro, los tres canales del parade están a la misma altura (±5 %).
- [ ] La piel cae cerca de la línea de tono de piel del vectorscopio.
- [ ] Los dos clips se ven iguales en pantalla dividida antes del look.
- [ ] Existen 4 nodos con nombre y puedes apagar cada uno (Ctrl+D) para ver su efecto.

## Solución

> [!success]- Si la imagen queda muy contrastada tras la LUT
> Baja la exposición/ganancia en el nodo **Balance** (antes de la LUT): la LUT recorta lo que le llega fuera de rango.

> [!success]- Dónde poner LUTs
> En Resolve: Project Settings > Color Management > Lookup Tables > Open LUT Folder, copia el archivo `.cube` y presiona Update Lists.

## Notas personales

_Qué me costó, errores que cometí, dudas._
