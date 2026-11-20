---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Edición de video
herramienta: DaVinci Resolve
tags: [edicion-video, laboratorio]
---
# Lab 1 - Primer proyecto organizado en DaVinci Resolve

Parte de: [[33-Edicion-Video/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

**Cubre:** [[DaVinci Resolve (versión gratuita)]] · [[Proyecto, bins y línea de tiempo]] · [[Proxies y rendimiento al editar]] · [[Organizar material de video]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear un proyecto con estructura de carpetas, bins, línea de tiempo bien configurada y proxies, usando clips del iPhone.

## Pasos

1. Crea en la PC la estructura de carpetas del proyecto y copia ahí los clips de una práctica de grabación (de Video y audio).
2. En Resolve crea un proyecto nuevo y define antes de importar: resolución 1920×1080 (o 3840×2160) y cadencia de la mayoría de clips.
3. En la página Media importa los clips y crea bins: A-roll, B-roll, Audio, Música.
4. Genera **proxies** para los clips HEVC (clic derecho > Generate Proxy Media).
5. Crea una línea de tiempo y arma un corte simple de 30 s usando I/O y J/K/L.
6. Cierra Resolve, mueve un clip de carpeta, vuelve a abrir y repara el enlace (Relink).

## Criterios de terminado

- [ ] El proyecto tiene al menos 3 bins con clips.
- [ ] La línea de tiempo tiene la resolución y fps que elegiste antes de importar (Timeline > Settings).
- [ ] Los clips HEVC muestran ícono de proxy y la reproducción es fluida.
- [ ] Reparaste un clip offline con Relink y vuelve a mostrarse.

## Solución

> [!success]- Si no aparece la opción de proxy
> En versiones recientes está en clic derecho > **Generate Proxy Media**; en versiones anteriores, **Generate Optimized Media**. Activa Playback > Use Proxy Media if Available.

## Notas personales

_Qué me costó, errores que cometí, dudas._
