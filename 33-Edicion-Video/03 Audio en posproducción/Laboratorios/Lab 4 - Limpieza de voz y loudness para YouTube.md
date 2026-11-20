---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Edición de video
herramienta: Audacity + DaVinci Resolve (Fairlight)
tags: [edicion-video, laboratorio]
---
# Lab 4 - Limpieza de voz y loudness para YouTube

Parte de: [[33-Edicion-Video/03 Audio en posproducción/_Índice Audio en posproducción|Audio en posproducción]]

**Cubre:** [[Limpieza de ruido en audio]] · [[Ecualización y compresión de voz]] · [[Loudness (LUFS) por plataforma]] · [[Mezcla de voz y música]] · [[Audacity]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Tomar una voz grabada con el [[DJI Mic Mini 2]] (idealmente la toma exterior sin cancelación del Lab 5 de Video y audio), limpiarla y entregar una mezcla con música lista para YouTube.

## Pasos

1. Exporta la voz como WAV 48 kHz y ábrela en Audacity.
2. Selecciona unos segundos de solo ruido > Efecto > Reducción de ruido > Obtener perfil; luego selecciona todo y aplica con reducción moderada.
3. Regresa el WAV a Resolve y sustituye la pista.
4. En Fairlight aplica EQ (pasa altos ~80 Hz) y un compresor moderado a la voz.
5. Agrega música libre de regalías y haz ducking cuando hay voz.
6. Mide el loudness integrado del programa completo y ajusta hacia la referencia de YouTube (verificar; las plataformas los cambian), con un limitador para que el true peak no pase de −1 dBTP.
7. Exporta y escucha con audífonos y con el celular.

## Criterios de terminado

- [ ] Comparando antes/después, el ruido de fondo se escucha claramente más bajo sin voz robótica.
- [ ] La música baja cuando hay voz y la voz se entiende siempre.
- [ ] El medidor de Fairlight marca un loudness integrado cercano al objetivo elegido (±1 LU) y true peak ≤ −1 dBTP.
- [ ] Anotaste los valores finales (LUFS, true peak) en la nota del proyecto.

## Solución

> [!success]- Valores de partida
> - Reducción de ruido en Audacity: 6–12 dB, sensibilidad media.
> - Compresor: ratio 3:1, ataque rápido, liberación media, 3–6 dB de reducción en las partes fuertes.
> - Música bajo voz: 15–20 dB por debajo.
> - Objetivo: alrededor de −14 LUFS para YouTube (verificar; las plataformas los cambian).

## Notas personales

_Qué me costó, errores que cometí, dudas._
