---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
contenido: completo
---
# Presets y LUTs en fotografía

Parte de: [[32-Edicion-Foto/06 Look y técnicas/_Índice Look y técnicas|Look y técnicas]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **preset** (en darktable se llama **estilo**; en RawTherapee, **perfil de procesamiento**) es un conjunto guardado de ajustes de edición que puedes aplicar a otras fotos con un clic: exposición, curva, HSL, gradación, grano, etc. Una **LUT** (*Look-Up Table*, tabla de consulta) es otra cosa: un archivo que dice, para cada color de entrada, qué color de salida debe tener. Una LUT 3D (`.cube`, `.3dl` o una imagen *HaldCLUT* `.png`) transforma el color de forma fija, sin saber nada de exposición, máscaras o contenido.

### Cómo funciona
- **Preset/estilo**: es una receta de parámetros. En darktable, desde el cuarto oscuro, *crear estilo a partir del historial* (o en la mesa de luz, módulo *estilos*); eliges qué módulos incluir. Luego lo aplicas a una o mil fotos, al importar o al exportar. Como son parámetros, puedes ajustarlos después foto por foto.
- **LUT 3D**: una rejilla (por ejemplo, 33 × 33 × 33 puntos) con un color de salida para cada combinación RGB; los valores intermedios se interpolan. Viene del video, donde se usan para convertir material Log a Rec. 709 o para dar un look (ver [[Material Log y LUTs]]). En foto se aplica con el módulo **lut 3D** de darktable, la *simulación de película* de RawTherapee (HaldCLUT) o capas de ajuste en Photopea.
- **Orden**: una LUT espera una entrada concreta (un espacio de color y un brillo). Aplícala **después** de corregir exposición y balance, y respeta el espacio para el que se creó.

### Por qué un preset descargado no se ve igual en todas tus fotos
Porque el preset se diseñó sobre **otras fotos**: otra luz, otra exposición, otro balance de blancos, otra cámara y otro perfil de color. Aplicar "+0.5 EV y sombras azules" a una foto ya clara o tomada bajo luz de tungsteno da algo muy distinto que en la foto de muestra. Además, muchos presets de Lightroom no se traducen a darktable, porque los motores son diferentes. Por eso siempre hay que **corregir primero** cada foto (exposición, blancos, negros, balance) y luego aplicar el look, y ajustar la intensidad.

### Ejemplo
Quieres un look cálido y mate para tus fotos de viaje con la a6700:
1. Revela una foto representativa: corrige exposición y balance.
2. Aplica solo los ajustes del look: *balance de color rgb* (luces cálidas, sombras verde azulado), *curva rgb* con negros levantados, *grain* fino.
3. Crea el estilo "Viaje cálido" incluyendo **solo esos módulos**, no exposición ni recorte.
4. En la mesa de luz, selecciona las demás fotos (ya corregidas) y aplica el estilo.

Para usar una LUT `.cube` gratis que imita una película: módulo *lut 3D*, elige el archivo, define el espacio de color de entrada que indique su autor y baja la opacidad del módulo al 50–70 %.

### Cuándo usarlo y cuándo no
- **Úsalos** para acelerar series, mantener una identidad visual y como punto de partida.
- **No** como sustituto de la corrección básica, ni con fotos de luz muy distinta sin ajustar.
- **Haz los tuyos**: un estilo propio, hecho con tus fotos, funciona mucho mejor que uno comprado.

### Errores comunes
1. **Presets que incluyen exposición y balance**: rompen fotos que ya estaban bien. Separa "corrección" de "look".
2. **LUT de video sobre foto normal** (una LUT pensada para S-Log3): colores muy contrastados y saturados.
3. **LUT al 100 %**: suele ser demasiado; dosifica con la opacidad.
4. **Comprar paquetes** creyendo que harán buenas fotos de fotos mal expuestas.

### Para profundizar
- [darktable 5.6: módulos de procesamiento (lut 3D, color balance rgb)](https://docs.darktable.org/usermanual/5.6/en/module-reference/processing-modules/)
- [RawPedia: crear perfiles de procesamiento de uso general](https://rawpedia.rawtherapee.com/Creating_processing_profiles_for_general_use)
- [[Consistencia de color en una serie]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un preset o estilo?
> Un conjunto guardado de ajustes que aplicas con un clic. En darktable se llaman **estilos**.

> [!question]- ¿Qué es una LUT?
> Una tabla que transforma colores de entrada en colores de salida (archivos `.cube`). darktable la aplica con el módulo **lut 3D**.

> [!question]- ¿Por qué un preset descargado no se ve igual en todas tus fotos?
> Cada foto tiene distinta exposición, balance y luz. El preset es un punto de partida; siempre ajustas después.

## Práctica

- [[Práctica Edición 09 - Recrear un look y aplicarlo a una serie]]

