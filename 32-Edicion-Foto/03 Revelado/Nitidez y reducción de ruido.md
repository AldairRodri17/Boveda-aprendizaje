---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
contenido: completo
---
# Nitidez y reducción de ruido

Parte de: [[32-Edicion-Foto/03 Revelado/_Índice Revelado|Revelado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **ruido** es la variación aleatoria de brillo y color que aparece en fotos con ISO alto o sombras levantadas. Hay dos tipos: **ruido de luminancia** (grano, como arena) y **ruido de crominancia** (manchas de colores, sobre todo en sombras). La **nitidez** (*sharpening*) aumenta el contraste en los bordes para que los detalles se vean más definidos. Ambos se tratan en el revelado y están ligados: la nitidez amplifica todo lo que parezca un borde, incluido el ruido.

### Cómo funciona
**Orden: primero reduce el ruido, después aplica nitidez.** Si enfocas primero, el algoritmo realza los granos como si fueran detalle y luego es mucho más difícil quitarlos sin borrar textura real.

- **Ruido de crominancia**: se reduce casi sin costo, porque el ojo es poco sensible al detalle de color; promediar el color en zonas pequeñas elimina las manchas sin que se note pérdida.
- **Ruido de luminancia**: reducirlo implica suavizar, y con eso se va textura fina (poros, hojas, tela). Hay que dosificar.
- **Nitidez**: técnicas como la *máscara de enfoque* (*unsharp mask*) o la deconvolución. Se ajusta con radio (tamaño del borde), cantidad y umbral (no enfocar zonas lisas).

En **darktable**:
- **denoise (profiled)**: usa perfiles de ruido medidos para cada modelo de cámara e ISO. Tiene modos para luminancia y crominancia por separado (Pendiente de confirmar que tu a6700 tenga perfil; si no, usa el modo genérico).
- **diffuse or sharpen** (difundir o enfocar): presets como *sharpen sensor demosaicing* o *lens deblur*; más potente pero más lento. El módulo **sharpen** clásico también existe.
- **local contrast**: no es nitidez de detalle sino "claridad".
- En 5.6, el módulo opcional **neural restore** hace reducción de ruido con IA (ver [[Reducción de ruido con IA]]).

En **RawTherapee**: *Detalle > Reducción de ruido* y *Enfoque* (con deconvolución RL o máscara de enfoque) y *Nitidez en microcontraste*.

### Ejemplo
Concierto con la a6700 a ISO 12800:
1. *denoise (profiled)*: modo con crominancia fuerte y luminancia moderada.
2. Revisa al 100 % la cara del cantante: si parece cera, baja la luminancia.
3. *diffuse or sharpen > sharpen sensor demosaicing*, o *sharpen* con cantidad baja.
4. Añade un poco de grano (*módulo grain*) si la piel quedó demasiado lisa: el grano fino se ve más natural que el ruido digital.

**iPhone 15 Pro**: las fotos HEIF ya salen con reducción de ruido y nitidez aplicadas por el procesamiento computacional, así que **casi no necesitan nitidez extra**; añadir más produce halos. Los ProRAW también llevan parte de ese procesamiento, de modo que basta con poca nitidez.

### Cuándo usarlo y cuándo no
- **Reducción de ruido**: ISO alto, sombras levantadas, astrofotografía, cielos lisos.
- **Nitidez de captura**: casi siempre un poco en RAW de cámara, porque los RAW salen algo suaves.
- **Nitidez de salida**: según destino; para web al tamaño final, para impresión más fuerte.
- **No enfoques** foto desenfocada o movida esperando que se arregle.

### Errores comunes
1. **Nitidez antes que reducción de ruido**.
2. **Juzgar en vista ajustada a la pantalla**: revisa al 100 %.
3. **Exceso de reducción de luminancia**: piel de plástico y paisajes de acuarela.
4. **Halos** blancos en bordes por radio grande o cantidad alta.

### Para profundizar
- [darktable 4.6: denoise (profiled)](https://docs.darktable.org/usermanual/4.6/en/module-reference/processing-modules/denoise-profiled/)
- [darktable 5.6: flujo y correcciones recomendadas](https://docs.darktable.org/usermanual/5.6/en/overview/workflow/process/)
- [[ISO y ruido digital]] en el temario de fotografía.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué se reduce el ruido antes de aplicar nitidez?
> Porque la nitidez acentúa todo, incluido el ruido. Primero limpias, luego afilas.

> [!question]- ¿Qué tipo de ruido se reduce casi sin costo?
> El de **crominancia** (manchas de color). El de luminancia se reduce con cuidado para no dejar la piel o el follaje "plásticos".

> [!question]- ¿Qué módulos usas en darktable?
> **denoise (profiled)** (reducción de ruido con perfiles de la cámara) y **diffuse or sharpen** o **sharpen** para nitidez; **local contrast** para microcontraste.

> [!question]- ¿Necesitan las fotos del iPhone mucha nitidez extra?
> En general no: el procesado de Apple (incluso en ProRAW) ya aplica nitidez y reducción de ruido. Ajusta poco y revisa al 100 %.

## Práctica

- [[Práctica Edición 04 - Revelado completo paso a paso]]

