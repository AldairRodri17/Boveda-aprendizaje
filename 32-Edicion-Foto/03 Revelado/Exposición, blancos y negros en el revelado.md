---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Exposición, blancos y negros en el revelado

Parte de: [[32-Edicion-Foto/03 Revelado/_Índice Revelado|Revelado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Es el primer ajuste real de cualquier revelado. La **exposición** sube o baja el brillo de toda la imagen de forma proporcional, en pasos (EV): +1 EV duplica la luz, −1 EV la reduce a la mitad, igual que si hubieras cambiado la apertura o la velocidad al disparar. Los **puntos de blanco y negro** definen qué valor de la escena se convierte en el blanco más brillante y en el negro más profundo que se van a mostrar; entre ambos queda todo el rango tonal de la foto. Bien ajustados, la imagen tiene contraste completo sin perder detalle en los extremos.

### Cómo funciona
En un RAW, la exposición es casi "gratis": la cámara guardó 12–14 bits por canal, así que subir +1 EV en el revelado se parece mucho a haber expuesto más (aunque también sube el ruido de las sombras). Lo que sí no se recupera son las **altas luces recortadas**: si el sensor se saturó, no hay dato.

En darktable el trabajo se divide:
1. **Módulo exposición**: llevas los **medios tonos** (la piel, el pasto, una pared gris) al brillo que quieres. Por defecto aplica unos +0.7 EV a los RAW para compensar que las cámaras subexponen para proteger luces.
2. **Mapeo de tonos (filmic rgb, sigmoid o AgX)**: aquí defines los puntos de blanco y negro. En filmic, *exposición relativa del blanco* y *del negro* (en EV respecto al gris medio) dicen cuántos pasos de luz caben arriba y abajo. Los selectores de color (pipetas) los calculan desde la propia imagen.
3. **Ecualizador de tonos**: si aún quieres bajar luces o subir sombras por zonas de brillo, es el sustituto de los sliders "Altas luces/Sombras" de Lightroom.

En Lightroom móvil los equivalentes son Exposición, Blancos y Negros; en RawTherapee, *Exposición* con *Compensación*, *Compresión de altas luces* y *Negro*.

### Ejemplo
Paisaje con la a6700 a contraluz, expuesto para no quemar el cielo, con primer plano muy oscuro:
- *Exposición*: +1.3 EV hasta que el pasto se ve natural (el cielo se ve quemado en pantalla, pero el dato sigue ahí).
- *filmic rgb > escena*: pipeta automática; resulta blanco relativo ≈ +4.5 EV y negro ≈ −7 EV. El cielo vuelve a tener nubes.
- *Ecualizador de tonos*: −0.5 EV en la zona de −2 EV para dar más peso al cielo.

### Cómo verificar que no quemaste ni empastaste
- **Histograma**: la curva no debe estar pegada a la derecha (luces quemadas) ni a la izquierda en una pared alta (sombras empastadas) salvo que sea intencional, como un fondo negro de estudio (ver [[Histograma y rango dinámico]]).
- **Indicadores de recorte**: en darktable, el botón de *sobreexposición/subexposición* bajo la imagen (atajo `O`) pinta las zonas recortadas; también el indicador de *recorte RAW* muestra si el sensor se saturó. En Lightroom, mantén presionado el slider de Blancos o Negros con dos dedos.
- **Mira al 100 %** las zonas brillantes (nubes, piel con brillo) y las sombras.

### Cuándo usarlo y cuándo no
Siempre al principio, antes de color y contraste local. Si la foto es un JPEG o HEIF, ve con cuidado: el margen es mucho menor y +1 EV ya puede revelar bandas en el cielo. Para el iPhone 15 Pro, dispara ProRAW si la escena es de alto contraste.

### Errores comunes
1. **Usar el contraste para arreglar la exposición**: primero exposición, luego contraste.
2. **Bajar Altas luces al máximo y subir Sombras al máximo**: resultado plano, gris y con halos ("efecto HDR").
3. **Ajustar con un monitor muy brillante**: las fotos salen oscuras en otras pantallas (ver [[Calibración del monitor]]).
4. **Apilar varios módulos de tono** en darktable (curva base + filmic + sigmoid): usa uno solo.

### Para profundizar
- [darktable 5.6: módulo exposure](https://docs.darktable.org/usermanual/5.6/en/module-reference/processing-modules/exposure/)
- [darktable 5.6: filmic rgb](https://docs.darktable.org/usermanual/5.6/en/module-reference/processing-modules/filmic-rgb/)
- [darktable 5.6: ecualizador de tono](https://docs.darktable.org/usermanual/5.6/es/module-reference/processing-modules/tone-equalizer/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué ajusta el control de exposición en el revelado?
> La luminosidad global, en pasos (EV). +1 EV duplica la luz de toda la imagen.

> [!question]- ¿Qué son los puntos de blanco y negro?
> Los límites del rango tonal: hasta dónde llega el tono más claro y el más oscuro. Ajustarlos da contraste sin perder detalle.

> [!question]- En darktable, ¿qué módulo hace este trabajo además de exposición?
> **filmic rgb** o **sigmoid**: definen cómo se comprime el rango de la escena (relativo a blanco y negro) en la pantalla. El **ecualizador de tonos** (tone equalizer) ajusta zonas de luminosidad.

> [!question]- ¿Cómo verificas que no quemaste altas luces ni empastaste sombras?
> Con el histograma y los indicadores de recorte (en darktable, los avisos de sobre/subexposición). Un poco de negro puro puede ser intencional; blancos quemados en cara o cielo casi nunca.

## Práctica

- [[Práctica Edición 02 - Primer revelado de un ProRAW en darktable]]
- [[Práctica Edición 04 - Revelado completo paso a paso]]

