---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de video
tags: [edicion-video]
contenido: completo
---
# Organizar material de video

Parte de: [[33-Edicion-Video/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Organizar el material es definir **dónde viven los archivos, cómo se llaman y cómo se respaldan** antes de abrir el editor. Un editor solo guarda referencias a los archivos, así que una estructura estable evita clips *offline*, material perdido y horas buscando "esa toma del dron".

### Cómo funciona: estructura de carpetas
Una carpeta por proyecto con el mismo esqueleto siempre:

```text
D:\Video\2026-11-08 Guanajuato\
  01 Original\
     a6700\  iPhone\  Pocket4P\  MiniDron\  Audio-MicMini2\
  02 Convertidos\      (DNxHR/ProRes si hiciste transcodificación)
  03 Proxies\
  04 Música y SFX\     (con el PDF o captura de cada licencia)
  05 Gráficos\         (logos, miniatura, PSD/XCF)
  06 Proyecto\         (exportaciones del proyecto .drp, SRT)
  07 Exportados\       (máster y versiones por plataforma)
```

Reglas:
- Fecha al inicio con formato `AAAA-MM-DD` para que se ordene sola.
- La carpeta `01 Original` es **de solo lectura en tu cabeza**: no se renombra ni se borra nada ahí hasta archivar.
- Un **SSD** para el proyecto activo y un **disco de respaldo** distinto (ver [[SSD externo para grabar y respaldar]]).

### Por qué copiar a la PC antes de editar
- Las tarjetas y el almacenamiento de la cámara son **lentos** para editar y se dañan con el uso.
- Si editas desde la tarjeta y la formateas, el proyecto queda sin material.
- Copiar la carpeta completa (no solo los MP4) conserva metadatos: la a6700 guarda XML con datos de giroscopio que usa Gyroflow ([[Estabilización en posproducción (Gyroflow)]]); el Osmo Pocket 4P guarda los archivos en su memoria interna de 103 GB, que debes vaciar.
- Haz la copia con verificación (por ejemplo, la opción *Clone* de la página Media de Resolve, que verifica con suma de comprobación) y **luego** respaldo en un segundo disco. Regla mínima: 2 copias en 2 discos antes de formatear tarjetas.

### Ejemplo: 300 clips sin nombre después de un viaje
1. Copia cada tarjeta a su subcarpeta en `01 Original` y respalda.
2. Importa todo a Resolve en bins por cámara (arrastrar la carpeta crea bins automáticos).
3. Revisa rápido con J-K-L y califica: marca favoritos con *Flags* o colores (verde = usable, rojo = descartar), escribe palabras clave en *Keywords* ("mercado", "callejón", "dron atardecer").
4. Crea **Smart Bins** por palabra clave o por cámara.
5. Si conviertes 10 bits a DNxHR, guarda los resultados en `02 Convertidos` con el mismo nombre del original.
6. Solo entonces empiezas la línea de tiempo.

No renombres los archivos uno por uno en Windows: pierdes la relación con los metadatos de la cámara; mejor usa los campos de nombre de clip y metadatos dentro del editor.

### Cuándo aplicarlo
Siempre, incluso en un reel de 30 s: con la misma estructura, dentro de un año encontrarás el material y la licencia de la música en segundos. En proyectos de un solo clip basta con la carpeta del proyecto y `07 Exportados`.

### Errores comunes
- **Editar desde Descargas o el escritorio**: son carpetas que limpias, que se sincronizan con la nube o que Windows mueve; un día los clips quedan *offline* o desaparecen.
- **Formatear la tarjeta con una sola copia**: si el disco falla, se perdió el viaje.
- **Mezclar proyectos en una misma carpeta "Videos"**: imposible archivar o respaldar por proyecto.
- **No guardar las licencias de música**: luego no puedes disputar un reclamo ([[Content ID y derechos en plataformas]]).

### Para profundizar
- [Capacitación oficial de DaVinci Resolve (página Media y Clone)](https://www.blackmagicdesign.com/products/davinciresolve/training)
- [Manual de Kdenlive: administración del proyecto](https://docs.kdenlive.org/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué estructura de carpetas conviene para un proyecto?
> Una carpeta por proyecto con subcarpetas: `01_Video`, `02_Audio`, `03_Musica`, `04_Graficos`, `05_Proyecto`, `06_Exportes`. Nombra con fecha: `2026-11-15_ReelCafe`.

> [!question]- ¿Por qué copiar el material a la PC antes de editar?
> El iPhone o la tarjeta pueden desconectarse, y necesitas respaldo. Aplica respaldo 3-2-1: 3 copias, 2 medios distintos, 1 fuera de casa.

> [!question]- Caso: después de un viaje tienes 300 clips sin nombre. ¿Cómo empiezas?
> Copia y respalda; luego en Resolve crea bins por día o lugar y marca los mejores con color o favoritos antes de editar.

> [!question]- Error común: editar directamente desde la carpeta Descargas. ¿Qué riesgo tiene?
> Borrar sin querer los originales al limpiar la carpeta y que el proyecto quede offline.

## Práctica

- [[Lab 1 - Primer proyecto organizado en DaVinci Resolve]]

