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
# DaVinci Resolve (versión gratuita)

Parte de: [[33-Edicion-Video/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
DaVinci Resolve es el editor de Blackmagic Design que reúne en un solo programa edición, efectos, color, audio y exportación. La versión gratuita no es una demo: no caduca, no pone marca de agua y tiene la misma calidad de procesamiento que la versión de pago (Studio). Al revisarlo en noviembre de 2026 la versión vigente es la **21.1**; la 21 agregó una página **Photo** para editar fotos, además de varias herramientas de IA (la mayoría son de Studio).

### Cómo funciona
Resolve se organiza en **páginas**, una por etapa del trabajo. Las recorres de izquierda a derecha en la barra inferior:

```mermaid
flowchart LR
    A[Media<br/>importar y organizar] --> B[Cut / Edit<br/>armar la historia]
    B --> C[Fusion<br/>gráficos y composición]
    C --> D[Color<br/>corrección y gradación]
    D --> E[Fairlight<br/>mezcla de audio]
    E --> F[Deliver<br/>exportar]
```

- **Media**: navegas tus discos, importas y organizas en bins (ver [[Proyecto, bins y línea de tiempo]]).
- **Cut**: edición rápida, pensada para videos cortos y redes. **Edit**: la línea de tiempo clásica con todas las herramientas.
- **Fusion**: composición por nodos para títulos animados y efectos ([[Motion graphics básico (Fusion)]]).
- **Color**: corrección y gradación por nodos ([[Nodos de color en DaVinci Resolve]]).
- **Fairlight**: mezcla, EQ, compresión y medición de loudness.
- **Deliver**: presets de exportación (YouTube, H.264, H.265, ProRes…).

Un detalle clave: Resolve **no copia** tus archivos al importarlos; guarda en la base de datos del proyecto una referencia a la ruta. Si mueves o borras el original, el clip aparece en rojo como *Media Offline* y tienes que usar **Relink** para volver a apuntarlo.

### Qué te limita la versión gratuita (y por qué importa con tu equipo)
Según la página oficial de Studio, lo exclusivo de la versión de pago incluye:
- Exportar por encima de **UHD (3840×2160) y 60 fps**. La gratuita entrega hasta UHD a 60 fps, de sobra para YouTube y redes.
- **Formatos de 10 bits** en H.264/H.265 y la **decodificación y codificación por hardware** de H.264/H.265 (la GPU no ayuda a reproducir esos archivos en la gratuita).
- IA: aislamiento de voz, Magic Mask/aislamiento de objetos, Speed Warp, Super Scale, reencuadre inteligente, reducción de ruido de video avanzada, entre otras.
- Varias GPU, HDR Dolby Vision/HDR10+ y scripting con Python.

Esto choca con tu material. En Windows, la gratuita **no abre H.264/H.265 de 10 bits** (en los foros oficiales es la causa típica de clips que solo muestran audio o salen *offline*):
- **Sony a6700** en XAVC S 4:2:2 10 bits o XAVC HS (H.265): no abren. XAVC S 4:2:0 8 bits sí.
- **iPhone 15 Pro** en HEVC HDR (10 bits): no abre. En **ProRes / Apple Log** sí, porque ProRes no es H.264/H.265.
- **Osmo Pocket 4P** graba MP4 HEVC; en **D-Log 2 / D-Log (10 bits)** no abre; en color estándar de 8 bits debería abrir (pruébalo con un clip).
- **DJI Mini 4K** graba H.264 de 8 bits a 100 Mbps: abre sin problema.

La salida es convertir esos clips a un códec intermedio (DNxHR HQX o ProRes 422) con una herramienta gratuita como Shutter Encoder antes de importarlos, o editarlos en Kdenlive. Lo detallo en [[Proxies y rendimiento al editar]]. Tu GTX 1660 Ti (6 GB) cumple el mínimo de 4 GB de VRAM y CUDA que pide la 21.1, con driver NVIDIA Studio reciente.

### Ejemplo
Video de un fin de semana con la a6700 en S-Cinetone 8 bits, tomas del dron y entrevistas con el [[DJI Mic Mini 2]]: importas en **Media**, armas en **Edit**, corriges en **Color**, mezclas la voz en **Fairlight** midiendo −14 LUFS y exportas en **Deliver** con el preset de YouTube a 4K. Todo eso cabe en la versión gratuita.

### Cuándo usarlo y cuándo no
Úsalo como editor principal: es el más completo de los gratuitos y lo que aprendas se traslada al mundo profesional (hay certificación oficial, ver [[Certificación de DaVinci Resolve (Blackmagic)]]). Si casi todo tu material es 10 bits y no quieres convertir, considera Studio (pago único) o [[Kdenlive y Shotcut]]. Para un reel rápido desde el celular, [[CapCut]] es más directo.

### Errores comunes
- **Mover la carpeta de material** después de importar: todo queda *offline*. Organiza antes ([[Organizar material de video]]).
- **Grabar en 10 bits sin saber que la gratuita no lo abre en Windows**: pruebas un clip antes del viaje, no después.
- **Usar la página Cut y Edit a la vez sin entender que comparten la misma línea de tiempo**: los cambios se ven en ambas.
- **Querer hacer todo en Edit**: el color fino se hace en Color y la mezcla en Fairlight; ahí están las herramientas buenas.

### Para profundizar
- [DaVinci Resolve (sitio oficial)](https://www.blackmagicdesign.com/products/davinciresolve)
- [Diferencias de DaVinci Resolve Studio](https://www.blackmagicdesign.com/products/davinciresolve/studio)
- [Capacitación oficial gratuita de Blackmagic](https://www.blackmagicdesign.com/products/davinciresolve/training)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué incluye la versión gratuita de DaVinci Resolve?
> Edición (páginas **Cut** y **Edit**), efectos y composición (**Fusion**), corrección de color (**Color**), audio (**Fairlight**) y exportación (**Deliver**). Es un programa profesional completo, con algunas funciones reservadas a la versión Studio (de pago).

> [!question]- ¿Qué limitaciones de la versión gratuita te pueden afectar?
> Exporta hasta **UHD (3840×2160) a 60 fps**; varias herramientas de IA y la reducción de ruido de video son de Studio; y la aceleración por hardware para decodificar H.264/H.265 está limitada (en Windows puede ir más lenta con clips HEVC del iPhone). La lista exacta cambia por versión: revísala en la página de Blackmagic (Pendiente de confirmar para tu versión).

> [!question]- ¿Qué página usas para cada tarea?
> **Media**: importar y organizar. **Cut/Edit**: armar la historia. **Fusion**: gráficos animados. **Color**: corrección/gradación. **Fairlight**: mezcla de audio. **Deliver**: exportar.

> [!question]- Error común: borrar o mover los clips originales después de importarlos. ¿Qué pasa?
> Resolve **no copia** los archivos: solo los enlaza. Si los mueves, aparecen como *media offline* (rojos) y hay que volver a vincularlos (Relink).

## Práctica

- [[Lab 1 - Primer proyecto organizado en DaVinci Resolve]]

