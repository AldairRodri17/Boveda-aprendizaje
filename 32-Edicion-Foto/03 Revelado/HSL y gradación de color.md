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
# HSL y gradación de color

Parte de: [[32-Edicion-Foto/03 Revelado/_Índice Revelado|Revelado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**HSL** significa *Hue, Saturation, Luminance*: **tono** (qué color es), **saturación** (qué tan intenso) y **luminancia** (qué tan claro u oscuro). En edición, una herramienta HSL te deja modificar esos tres valores **por gama de color**: solo los azules, solo los naranjas, solo los verdes. La **gradación de color** (*color grading*) es otra cosa: teñir zonas tonales (sombras, medios tonos y luces) con un color para crear un ambiente o "look".

### Cómo funciona
- **HSL/Mezcla de color**: el programa identifica píxeles por su tono y aplica el cambio solo a ellos, con transición suave a los colores vecinos. En Lightroom se llama *Mezcla*; en darktable, el módulo **ecualizador de color** (*color equalizer*), con pestañas de tono, saturación y brillo por gama, más el **balance de color rgb** para saturación global; en RawTherapee, *Ecualizador HSV*; en Snapseed, se aproxima con *Selectivo*.
- **Gradación**: eliges un tono y una saturación para sombras, otro para medios y otro para luces, más un balance que decide dónde termina cada zona. En darktable está en **balance de color rgb > 4 vías** (*lift, gamma, gain* y potencia global); en Lightroom, *Gradación de color*.
- Complementarios: el contraste naranja/azul (piel cálida, sombras frías) es el clásico del cine porque son colores opuestos en el círculo cromático (ver [[Teoría del color y armonías]]).

### Ejemplo
Retrato con la a6700 en un parque, cielo pálido y piel un poco rojiza:
1. **Cielo más profundo sin tocar la piel**: en el ecualizador de color, gama **azul**: saturación +20 % y brillo −15 %. La piel está en la gama naranja, así que no se mueve. Si el cielo tiene cianes, ajusta también esa gama.
2. **Piel**: gama **naranja** (y un poco roja): tono ligeramente hacia amarillo para quitar rojez, saturación −5 a −10 %, brillo +5 %.
3. **Pasto amarillento**: gama verde/amarillo, tono hacia verde y saturación −10 %.
4. **Gradación**: en *balance de color rgb > 4 vías*, sombras (*lift*) hacia azul verdoso con croma baja; luces (*gain*) hacia naranja con croma baja. Sutil: si se nota a primera vista, es demasiado.

### Cuándo usarlo y cuándo no
- **HSL**: correcciones de color específicas (cielos, piel, vegetación, ropa que distrae), y la base de una buena conversión a blanco y negro (ver [[Blanco y negro]]).
- **Gradación**: cuando ya está todo corregido y quieres dar un ambiente o unificar una serie.
- **No uses HSL para arreglar un mal balance de blancos**: corrige primero con calibración de color; luego HSL.

### Errores comunes
1. **Saturar la gama naranja**: la piel se vuelve zanahoria. Para piel, la luminancia del naranja es más útil que la saturación.
2. **Bajar mucho la luminancia del azul**: aparecen halos y ruido en el borde del cielo con edificios o árboles.
3. **Gradación demasiado fuerte**: piel verde o magenta; revisa siempre la piel después.
4. **Mover tono de forma extrema** en JPEG de 8 bits: bandas y manchas.

### Para profundizar
- [darktable 4.6: balance de color rgb](https://docs.darktable.org/usermanual/4.6/en/module-reference/processing-modules/color-balance-rgb/)
- [darktable 5.6: módulos de procesamiento](https://docs.darktable.org/usermanual/5.6/en/module-reference/processing-modules/)
- [[Lightroom móvil (versión gratuita)]] para practicar Mezcla y Gradación.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es HSL en edición?
> Ajustar por separado **tono**, **saturación** y **luminosidad** de cada gama de color (rojos, naranjas, verdes, azules...).

> [!question]- ¿Cómo haces que un cielo azul sea más profundo sin afectar la piel?
> Baja la luminosidad (y quizá sube algo la saturación) solo de los **azules**. En darktable puedes usar *color zones* o el **balance de color rgb**.

> [!question]- ¿Qué es la gradación de color (color grading)?
> Dar un tono a sombras, medios y luces por separado (p. ej. sombras frías y luces cálidas) para crear un ambiente.

> [!question]- ¿Qué gama ajustas para mejorar tonos de piel?
> Principalmente **naranjas** (y algo de rojos): luminosidad para aclarar, saturación con moderación.

## Práctica

- [[Práctica Edición 04 - Revelado completo paso a paso]]
- [[Práctica Edición 09 - Recrear un look y aplicarlo a una serie]]

