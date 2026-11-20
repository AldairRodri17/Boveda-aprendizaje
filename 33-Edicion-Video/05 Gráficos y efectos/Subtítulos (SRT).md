---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de video
tags: [edicion-video]
---
# Subtítulos (SRT)

Parte de: [[33-Edicion-Video/05 Gráficos y efectos/_Índice Gráficos y efectos|Gráficos y efectos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un archivo **SRT** (SubRip Subtitle) es un archivo de texto plano con los subtítulos de un video y sus tiempos. Lo aceptan YouTube, Facebook, reproductores como VLC y todos los editores. Su ventaja es que es **independiente** del video: puedes corregir una falta o subir otro idioma sin volver a exportar.

### Cómo funciona: el formato
Cada subtítulo es un bloque con cuatro partes:
1. Número consecutivo (1, 2, 3…).
2. Tiempo de entrada y salida con formato `HH:MM:SS,mmm --> HH:MM:SS,mmm` (horas, minutos, segundos y **milisegundos separados con coma**).
3. Una o dos líneas de texto.
4. Una **línea en blanco** que separa del siguiente bloque.

```text
1
00:00:00,500 --> 00:00:03,800
Bienvenidos a este video
sobre impresión 3D.

2
00:00:04,200 --> 00:00:07,000
Hola a todos
```

Conversiones útiles:
- 4.2 s → `00:00:04,200`; 7 s → `00:00:07,000`.
- 95.75 s = 1 min 35.75 s → `00:01:35,750`.
- Duración de `00:01:05,300` a `00:01:08,100` = 68.1 − 65.3 = **2.8 s**.

Guarda el archivo en **UTF-8** para que los acentos y la ñ se vean bien.

### Quemados vs en archivo
| | Quemados (*burn-in*, *open captions*) | Archivo SRT (*closed captions*) |
|---|---|---|
| Dónde están | Dibujados en la imagen | Archivo aparte que la plataforma muestra |
| ¿Se pueden apagar? | No | Sí, el espectador decide |
| Estilo | Total control (fuente, color, animación) | Lo define el reproductor |
| Corregir | Reexportar el video | Subir otro SRT |
| Idiomas | Uno por versión | Varios archivos |
| Uso típico | Reels/TikTok (se ven sin sonido) | YouTube, cursos |

### Cómo hacerlos
- **Resolve**: *Effects → Titles → Subtitles* agrega una pista de subtítulos; también puedes **importar** un SRT al bin y arrastrarlo. En *Deliver → Subtitle Settings* eliges exportarlos como archivo SRT separado o quemarlos en la imagen. La transcripción automática del audio a subtítulos es función de Studio.
- **Kdenlive**: tiene editor de subtítulos y reconocimiento de voz local (Whisper o VOSK) gratuito para generar el primer borrador.
- **YouTube Studio**: genera subtítulos automáticos que puedes corregir y descargar como SRT.
Siempre **revisa y corrige** a mano: nombres propios, términos técnicos y puntuación.

### Pautas de legibilidad
- Máximo **2 líneas** por subtítulo y unos **42 caracteres por línea** (referencia común en la industria).
- Velocidad de lectura de unos **15 a 20 caracteres por segundo**; duración mínima ~1 s y máxima ~7 s.
- Corta las líneas en unidades de sentido (no separar artículo y sustantivo).
- Sincroniza la entrada con el inicio de la frase; no dejes un subtítulo encima de un cambio de plano si puedes evitarlo.
- En vertical, súbelos a la zona segura central ([[Títulos y lower thirds]]).

### Ejemplo
Tutorial de YouTube de 8 minutos: generas el borrador con Whisper en Kdenlive, lo corriges, exportas `tutorial.es.srt` y lo subes en YouTube Studio como subtítulos en español. Para el reel del mismo tema, quemas subtítulos grandes y con contorno en Resolve.

### Errores comunes
- **Usar punto en lugar de coma en los milisegundos** (`00:00:04.200`): el formato SRT define la coma; muchos reproductores y plataformas no reconocen la línea de tiempo y el subtítulo no aparece, o rechazan el archivo (el punto es del formato WebVTT).
- **Olvidar la línea en blanco** entre bloques: se unen dos subtítulos.
- **Guardar en ANSI**: acentos rotos.
- **Subtítulos de 3 líneas o que duran medio segundo**: nadie alcanza a leerlos.

### Para profundizar
- [SubRip / formato SRT (referencia)](https://en.wikipedia.org/wiki/SubRip)
- [Manual de Kdenlive (subtítulos)](https://docs.kdenlive.org/)
- [Capacitación oficial de DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve/training)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe el bloque SRT #2 que va de 4.2 s a 7 s con el texto "Hola a todos".
> ```
> 2
> 00:00:04,200 --> 00:00:07,000
> Hola a todos
> ```

> [!question]- Un subtítulo va de `00:01:05,300` a `00:01:08,100`. ¿Cuánto dura?
> 68.1 − 65.3 = **2.8 s**.

> [!question]- Convierte 95.75 s a tiempo SRT.
> `00:01:35,750`

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un archivo SRT?
> Un archivo de texto con subtítulos numerados, cada uno con tiempo de inicio y fin (`00:00:01,000 --> 00:00:03,500`) y el texto.

> [!question]- ¿Qué diferencia hay entre subtítulos "quemados" y en archivo SRT?
> **Quemados** (burned-in): forman parte de la imagen, siempre visibles (típico en reels). **SRT** se sube aparte: el espectador los activa o desactiva y las plataformas los indexan.

> [!question]- ¿Qué pautas básicas de legibilidad sigues?
> Máximo 2 líneas, ~35–42 caracteres por línea, y suficiente tiempo en pantalla (más o menos 1 s mínimo por subtítulo corto).

> [!question]- Error común: usar punto en lugar de coma en los milisegundos del SRT. ¿Qué pasa?
> El formato SRT usa **coma** (`00:00:01,500`); algunos programas no leen el archivo con punto.

## Práctica

- [[Lab 6 - Subtítulos SRT y lower third]]

