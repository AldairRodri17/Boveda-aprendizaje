---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Video y audio
tags: [video-audio]
---
# Códecs y contenedores de video (H.264, H.265 y ProRes)

Parte de: [[31-Video-Audio/01 Técnica de video/_Índice Técnica de video|Técnica de video]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **códec** (codificador-decodificador) es el algoritmo que **comprime** el video para que quepa en la tarjeta y lo **descomprime** al reproducirlo. Un **contenedor** es el **archivo** que guarda ese video comprimido junto con el audio, los metadatos y a veces subtítulos. Ejemplo: un `.mov` del iPhone (contenedor) puede llevar video HEVC o ProRes (códec) y audio AAC o PCM.

### Cómo funciona
**Los contenedores** más comunes son **MP4** (`.mp4`, universal, el que pide YouTube) y **MOV** (`.mov`, de Apple, muy usado en cámaras y en edición). El contenedor no decide la calidad; el códec sí.

**Los códecs** se dividen en dos familias:

| Códec | Tipo | Ventaja | Costo |
|---|---|---|---|
| **H.264 (AVC)** | Long-GOP (entre cuadros) | Compatible con todo | Archivos medianos |
| **H.265 (HEVC)** | Long-GOP | ~misma calidad con menos bitrate; 10 bits | Más pesado de decodificar |
| **ProRes** | Intra-cuadro | Edición fluida, mucha calidad, 10 bits 4:2:2 | Archivos enormes |

- **Long-GOP** (*group of pictures*): guarda un cuadro completo cada cierto tiempo y, en los demás, solo **lo que cambió**. Muy eficiente, pero para mostrar un cuadro el editor tiene que reconstruirlo a partir de otros: por eso tu PC sufre al mover el cursor en la línea de tiempo.
- **Intra-cuadro** (ProRes, XAVC S-I de Sony): cada cuadro se comprime **por separado**. Más bits por segundo, pero cada cuadro se decodifica solo, así que la edición es ligera para el procesador (pero exige disco rápido).

```mermaid
flowchart TB
    subgraph LG["Long-GOP (H.264 / H.265)"]
      I1["I (completo)"] --> P1["P (cambios)"] --> B1["B (cambios)"] --> P2["P (cambios)"]
    end
    subgraph IN["Intra (ProRes / XAVC S-I)"]
      A1["Cuadro completo"] --> A2["Cuadro completo"] --> A3["Cuadro completo"]
    end
```

Apple indica que los archivos ProRes del iPhone pueden ser hasta **30 veces** más grandes que los HEVC: ProRes casi no descarta información, HEVC sí (de forma inteligente).

### Ejemplo
Tu equipo y sus códecs:
- **[[iPhone 15 Pro para foto y video]]**: HEVC, H.264 y ProRes. ProRes interno depende de la capacidad (128 GB: solo 1080p a 30/25 fps; 256 GB o más: hasta 4K a 30 fps); 4K a 60 fps en ProRes solo grabando a un disco externo por USB‑C.
- **[[Sony a6700]]**: **XAVC S** (H.264, 8 bits 4:2:0 o 10 bits 4:2:2), **XAVC HS** (H.265, 10 bits) y **XAVC S-I** (intra-cuadro 4:2:2 de 10 bits, hasta 600 Mbps en 4K 59.94p).
- **[[DJI Osmo Pocket 4P]]**: H.264/H.265 en MP4 (Pendiente de confirmar códecs exactos en la ficha de DJI).

**Caso: tu PC (GTX 1660 Ti) se traba con HEVC del iPhone en Resolve.** Opciones, de más a menos práctica:
1. Generar **proxies** u *optimized media* en Resolve y editar con ellos ([[Proxies y rendimiento al editar]]).
2. Transcodificar a un intermedio fácil de editar (DNxHR o ProRes) con Resolve o con [[HandBrake]] si solo quieres H.264 ligero.
3. Grabar en H.264 en lugar de HEVC si el proyecto es sencillo.
4. Revisar si tu versión de Resolve usa decodificación por GPU para HEVC; en la versión gratuita en Windows puede estar limitada (Pendiente de confirmar para tu versión).

### Cuándo usarlo y cuándo no
- **HEVC 10 bits**: el uso diario; buena calidad, archivos razonables, admite Log y HDR.
- **H.264**: cuando necesitas máxima compatibilidad o tu PC no puede con HEVC.
- **ProRes / intra**: trabajos importantes con [[Material Log y LUTs|Log]] y mucha corrección de color, siempre que tengas almacenamiento.
- **Para entregar** (YouTube, redes): H.264 o H.265 en MP4; ver [[Exportar video (códec, bitrate y formato por plataforma)]].

### Errores comunes
1. **Confundir contenedor con códec**: "es MP4" no dice nada de la calidad.
2. **Exportar en ProRes para YouTube "para que suba mejor"**: la plataforma recomprime todo; subes gigas sin ganar visiblemente. YouTube recomienda H.264 en MP4 con bitrate alto.
3. **Grabar ProRes sin espacio**: el iPhone se llena en minutos.
4. **Editar HEVC 10 bits 4:2:2 en una PC modesta** sin proxies.
5. **Mezclar códecs sin necesidad** en un mismo proyecto y luego no saber cuál es cuál.

### Para profundizar
- [Acerca de Apple ProRes en el iPhone (Apple)](https://support.apple.com/es-lamr/109041)
- [Apple ProRes white paper (Apple)](https://www.apple.com/cl/final-cut-pro/docs/Apple_ProRes.pdf)
- [Especificaciones de la Sony a6700 (Sony)](https://www.sony.co.in/interchangeable-lens-cameras/products/ilce-6700/spec)
- [Digital video concepts (MDN)](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_concepts)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre códec y contenedor?
> El **códec** es cómo se comprime la imagen (H.264, H.265/HEVC, ProRes). El **contenedor** es el "sobre" que guarda video, audio y metadatos (.mp4, .mov, .mkv). Un .mov puede llevar H.265 o ProRes.

> [!question]- ¿Por qué ProRes ocupa tanto más que H.265?
> ProRes es un códec **intra-cuadro** pensado para editar: comprime cada cuadro por separado y poco. H.264/H.265 son **inter-cuadro** (Long GOP): guardan cuadros completos de vez en cuando y el resto como diferencias, lo que ahorra mucho espacio pero exige más al procesador al editar.

> [!question]- Caso: tu PC se traba al editar clips HEVC del iPhone en Resolve. ¿Qué opciones tienes?
> Crear **proxies** u optimized media, transcodificar a un códec de edición, o bajar la resolución de reproducción. Grabar en ProRes también ayuda a la fluidez, pero llena el teléfono muy rápido.

> [!question]- Error común: "Si lo exporto en ProRes subo mejor calidad a YouTube". ¿Por qué no conviene?
> La plataforma recomprime todo a su propio formato. Un archivo ProRes enorme tarda mucho en subir y casi no gana nada frente a un H.264/H.265 con bitrate alto. ProRes sirve como **máster** o intermedio, no como entrega web.

## Práctica

- [[Lab 2 - Exposición de Apple Log con zebras y false color]]

