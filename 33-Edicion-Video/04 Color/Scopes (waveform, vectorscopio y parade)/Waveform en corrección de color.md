---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de video
tags: [edicion-video]
---
# Waveform en corrección de color

Parte de: [[Scopes (waveform, vectorscopio y parade)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **waveform** (monitor de forma de onda) grafica el **brillo** de la imagen: el eje horizontal corresponde a la posición horizontal en el cuadro (izquierda a derecha) y el eje vertical al nivel de brillo, de 0 (negro) a 100 (blanco) en IRE, o de 0 a 1023 en escala de 10 bits. Cada columna de píxeles de la imagen se dibuja como una columna de puntos.

### Cómo funciona
- Lo que está **arriba** en la gráfica es brillante; lo de **abajo**, oscuro.
- Lo que está **a la izquierda** del cuadro aparece a la izquierda de la gráfica. Si hay una ventana brillante en el lado derecho de la imagen, verás un grupo alto en el lado derecho del waveform.
- Si la traza **se aplana contra 100**, las altas luces están quemadas (sin detalle); si se aplana contra 0, las sombras están aplastadas.
- El **contraste** se ve como la distancia entre la parte más baja y la más alta de la traza.

```text
100 |      ____               <- cielo casi quemado
    |    /    \      ..
 60 |   |  ##  |   ....       <- piel (cara a la izquierda)
    |   |  ##  |  ......
  0 |___|______|_________     <- sombras
      izquierda   derecha
```

### Dónde suele estar la piel
Como referencia, la piel de un rostro bien expuesto suele quedar entre **40 y 70 IRE**: pieles claras más cerca de 60–70, pieles morenas alrededor de 45–60 y pieles oscuras más abajo. No es una regla fija: depende de la persona, de la luz y de la intención; lo importante es que la cara tenga detalle y se vea natural, y que se mantenga en el mismo nivel entre tomas de la misma escena.

Truco: aísla la cara con una ventana (Power Window) o mira solo la zona de la gráfica que corresponde a la posición horizontal de la cara.

### Ejemplo en Resolve
Toma de la a6700 en S-Cinetone a contraluz, con la persona a la izquierda:
1. Activa el waveform (Scopes en la página Color).
2. La zona de la cara está en 35 IRE: subexpuesta. Sube **Gamma** (medios) en las ruedas *Primaries* hasta que la cara llegue a ~55.
3. El cielo, a la derecha, ya toca 100: baja **Gain** un poco o usa la curva para recuperar altas luces.
4. Las sombras estaban en 8; las dejas ahí para conservar algo de profundidad.

### Errores comunes
- **Llevar siempre los negros a 0 y los blancos a 100**: no toda escena tiene un negro absoluto o un blanco puro. Una escena con niebla, un amanecer suave o una toma nocturna con poca luz se ven falsas si las estiras al rango completo; el contraste exagerado también saca ruido y banding en material de 8 bits. Usa el rango completo cuando la escena lo tenga, y respeta la intención.
- **Confundir 100 IRE con "correcto"**: las altas luces especulares (reflejos) pueden tocar 100; la piel nunca debería.
- **Leer el waveform con la vista de "Data levels" equivocada**: deja la escala en IRE si estás aprendiendo.
- **Ignorar la imagen**: un waveform perfecto con una cara oscura no sirve.

### Para profundizar
- [Página Color de DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve/color)
- [Capacitación oficial de DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve/training)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué muestra el waveform?
> El brillo de cada columna de la imagen: eje vertical de 0 (negro) a 100 (o 1023 en 10 bits) (blanco).

> [!question]- ¿Dónde suele estar la piel en el waveform?
> Para piel media, aproximadamente entre **50 y 70 %** (referencia, depende del tono de piel y de la intención).

> [!question]- Error común: llevar los negros a 0 y los blancos a 100 en todas las tomas. ¿Por qué no siempre?
> Una escena nocturna o con niebla no debe tener blancos a 100; la regla es no recortar sin querer, no estirar todo.

## Práctica

- [[Lab 5 - Corrección y gradación de Apple Log con scopes]]

