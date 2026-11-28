---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Fotografía
tags: [fotografia]
contenido: completo
---
# Enfoque manual y focus peaking

Parte de: [[30-Fotografia/03 Enfoque/_Índice Enfoque|Enfoque]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Enfoque manual (MF)** es girar tú el anillo de enfoque (o un deslizador en el teléfono) en lugar de dejarlo a la cámara. El **focus peaking** es una ayuda visual: la cámara colorea en vivo los bordes que están nítidos (rojo, amarillo, blanco), así ves de un vistazo dónde está el plano de foco. Su compañero es la **ampliación** (lupa o MF Assist): acercar la imagen en pantalla para afinar.

### Cómo funciona
El peaking detecta zonas de **alto contraste local** (bordes marcados) y las pinta. Cuando una zona está enfocada, sus bordes son más definidos y se iluminan; al desenfocarse, el color desaparece.

```mermaid
flowchart LR
  G[Giras el anillo] --> P[Peaking: los bordes nítidos se colorean]
  P --> Z[Amplías al 100 % para confirmar]
  Z --> D[Disparas]
```

Limitaciones: el peaking es aproximado. A aperturas cerradas (mucha profundidad de campo) se ilumina casi todo; con texturas suaves (piel, cielo) casi nada. Por eso, en tomas críticas, confirma con ampliación. El nivel de sensibilidad (bajo, medio, alto) y el color se ajustan en el menú; usa un color que contraste con la escena.

En la a6700 tienes:
- **MF** puro y **DMF** (enfocas con AF y retocas a mano con el anillo). Sony describe también un DMF de tiempo completo: girar el anillo en AF-S o AF-C pasa a manual.
- **Visualización de realce** (peaking) con nivel y color.
- **Ampliación** automática al girar el anillo, si la activas.

En el iPhone, la app nativa no tiene enfoque manual. **Blackmagic Camera** (gratuita, para video) ofrece enfoque manual con **focus peaking** y una ayuda de enfoque que resalta el detalle fino, además de zebras y falso color para exposición. Ver [[Blackmagic Camera (app)]]. Para fotos fijas, la cámara Profesional de [[Lightroom móvil (versión gratuita)]] permite fijar la distancia de enfoque a mano.

### Ejemplo
**Retrato nocturno con luces de fondo (a6700 + lente f/1.8)**: con poca luz el AF puede dudar. Cambia a MF, activa peaking en rojo y ampliación. Gira hasta que las pestañas se pinten, amplía sobre el ojo y afina. Dispara varias porque la persona se mueve unos milímetros.

**Producto o macro en mesa**: tripié, MF, ampliación al 100 % sobre la parte más importante (la etiqueta, el borde del anillo). Si necesitas todo nítido de adelante a atrás, mueve el foco en pasos y apila. Ver [[Focus stacking (apilado de enfoque)]].

**Video con la Osmo Pocket o el iPhone**: en Blackmagic Camera, prepara un cambio de foco (de una taza en primer plano a una persona al fondo). Marca ambas distancias con peaking y practica el giro del deslizador.

Ejercicio: a través de una ventana con malla o entre ramas, el AF enfocará la malla. Repite en MF con peaking y nota cómo puedes "atravesar" el obstáculo.

### Cuándo usarlo y cuándo no
**Mejor a mano cuando:**
- Hay poca luz o poco contraste (noche, cielo estrellado: enfoca en una estrella brillante ampliada).
- Hay obstáculos delante (vidrio, rejas, ramas).
- Macro y apilado, donde un milímetro importa.
- Tripié con varias tomas que deben compartir el mismo foco (HDR, panorámica, time-lapse).
- Video con cambios de foco planeados.
- Enfoque a la distancia hiperfocal en paisaje (ver [[Distancia hiperfocal]]).

**Mejor automático cuando:** sujetos que se mueven de forma impredecible (deporte, niños, fauna); ahí el AF con seguimiento de la a6700 es más rápido y preciso que tú.

### Errores comunes
1. **Confiar solo en el peaking a f/8 o más**: todo brilla y no sabes dónde está el plano exacto. Amplía.
2. **Olvidar regresar a AF**: la siguiente sesión sale toda desenfocada. Revisa el selector al empezar.
3. **Peaking demasiado sensible**: ensucia la imagen y engaña; bájalo a medio.
4. **Enfocar a infinito girando hasta el tope**: muchos lentes modernos van "más allá" de infinito; para estrellas enfoca con ampliación.

### Para profundizar
- [Blackmagic Camera (Blackmagic Design)](https://www.blackmagicdesign.com/products/blackmagiccamera)
- [Guía de ayuda α6700 (Sony)](https://helpguide.sony.net/ilc/2320/v1/en/)
- [Understanding Camera Autofocus (Cambridge in Colour)](https://www.cambridgeincolour.com/tutorials/camera-autofocus.htm)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es el focus peaking?
> Un resaltado de color sobre los bordes que están en foco. Te ayuda a enfocar a mano sin ampliar la imagen.

> [!question]- ¿Cuándo es mejor enfocar a mano?
> Macro, poca luz, estrellas, escenas con obstáculos delante (rejas, ramas), video con cambios de foco controlados y focus stacking.

> [!question]- ¿Qué apoyo de enfoque trae Blackmagic Camera en el iPhone?
> Enfoque manual con asistente de enfoque (resaltado tipo peaking) además de histograma y cebras, pero **solo para video**: la app no toma fotos fijas. Para enfocar a mano en foto usa la cámara Profesional de [[Lightroom móvil (versión gratuita)]] (deslizador de enfoque manual, además de velocidad, ISO y balance de blancos) u otra app de foto manual. Las opciones exactas dependen de la versión de cada app.

## Práctica

- [[Práctica Foto 04 - Enfoque manual, macro y apilado]]

