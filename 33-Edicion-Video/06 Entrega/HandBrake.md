---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de video
tags: [edicion-video]
---
# HandBrake

Parte de: [[33-Edicion-Video/06 Entrega/_Índice Entrega|Entrega]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
HandBrake es un **transcodificador de video gratuito y de código abierto** para Windows, macOS y Linux. Toma un video en casi cualquier formato y lo convierte a MP4, MKV o WebM con H.264, H.265 o AV1, usando presets listos para dispositivos y web. No edita: comprime y convierte.

### Cómo funciona
```mermaid
flowchart LR
    A[Abrir archivo o carpeta] --> B[Elegir preset<br/>p. ej. Fast 1080p30]
    B --> C[Ajustar: Video, Audio,<br/>Subtítulos, Dimensiones]
    C --> D[Cola de trabajos]
    D --> E[Archivo comprimido]
```
Conceptos clave:
- **Presets**: configuraciones para web, dispositivos y calidad (por ejemplo *Fast 1080p30*, *HQ 1080p30 Surround*, presets para Discord, YouTube, etc.).
- **Códec y codificador**: software (x264, x265, SVT-AV1: más lentos, mejor eficiencia) o **hardware** (NVENC de NVIDIA). Tu GTX 1660 Ti soporta **NVENC H.264 y H.265**, muy rápido; **no** codifica AV1 por hardware (eso llegó con la serie RTX 40).
- **Calidad constante (RF)**: en lugar de fijar un bitrate, defines la calidad y el tamaño sale variable. Menor número = mejor calidad y archivo más grande. Para x264 en 1080p, valores de 18 a 22 son un buen rango; para H.265 se usan números un poco más altos para calidad similar.
- **Bitrate promedio**: cuando necesitas un tamaño concreto (límite de subida); puedes activar *2-pass* para mejor reparto.
- **Cola**: agregas varios archivos y los procesa uno tras otro.

### Cuándo lo usarías en tu flujo
1. **Sacar versiones del máster** sin abrir el proyecto: del ProRes/DNxHR que exportaste ([[Exportar video (códec, bitrate y formato por plataforma)]]) a H.264 para YouTube, versión ligera para WhatsApp o para mandarle a un cliente.
2. **Aprovechar NVENC**: la versión gratuita de Resolve no codifica por hardware; HandBrake sí usa tu GPU.
3. **Reducir el tamaño** de un video para un límite de subida (correo, formularios, mensajería).
4. **Archivar** material de baja importancia en H.265 para ahorrar disco (no los originales de cámara de proyectos importantes).
No sirve para convertir a códecs de edición como DNxHR o ProRes; para eso usa Shutter Encoder ([[Proxies y rendimiento al editar]]).

### Ejemplo
Tienes un máster DNxHR HQ de 6 minutos (≈ varios GB) y debes subirlo a una plataforma con límite de 300 MB:
- Bitrate total máximo ≈ 300 × 8 / 360 ≈ 6.7 Mbps.
- En HandBrake: preset *Fast 1080p30*, pestaña Video → codificador **H.264 (NVEnc)**, *Avg Bitrate* 6,000 kbps; Audio AAC 160 kbps. Total ≈ 6.2 Mbps → ≈ 280 MB.
- Revisa el resultado completo antes de mandarlo.

### Errores comunes
- **Comprimir varias veces el mismo archivo**: cada compresión con pérdida descarta información; los artefactos (bloques, banding, pérdida de detalle) se acumulan generación tras generación. Comprime **siempre desde el máster** o el original, una sola vez por destino.
- **Cambiar la cadencia** en *Framerate* sin necesidad: deja *Same as source*.
- **RF demasiado alto** para ahorrar espacio en material del dron: cielos con bandas.
- **Usar H.265 o AV1 para alguien que no puede reproducirlos**: H.264 es el más compatible.

### Para profundizar
- [HandBrake (sitio oficial)](https://handbrake.fr/)
- [Documentación de HandBrake](https://handbrake.fr/docs/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve HandBrake?
> Programa gratuito y de código abierto para **convertir y comprimir** video (MP4/MKV/WebM con H.264, H.265, AV1).

> [!question]- ¿Cuándo lo usarías en tu flujo?
> Para reducir el tamaño de un máster para enviar por WhatsApp o correo, o convertir clips a un formato que tu editor abra mejor.

> [!question]- Error común: comprimir varias veces el mismo archivo. ¿Qué pasa?
> Cada compresión pierde calidad; convierte siempre desde el original o máster.

## Práctica

- [[Lab 8 - Exportar por plataforma]]

