---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de video
tags: [edicion-video]
---
# Vectorscopio

Parte de: [[Scopes (waveform, vectorscopio y parade)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **vectorscopio** es un gráfico circular que muestra el **color** de la imagen sin importar su brillo: el **ángulo** indica el matiz (rojo, amarillo, verde, cian, azul, magenta) y la **distancia al centro** indica la saturación. Sirve para controlar la saturación, detectar dominantes y revisar los tonos de piel.

### Cómo funciona
- El **centro** es el gris neutro: blanco, gris y negro sin color.
- Alrededor hay marcas (cajitas) para los colores primarios y secundarios: R, Mg, B, Cy, G, Yl. Su posición es fija por el estándar (Rec.709).
- Cada píxel se dibuja según su matiz y saturación; la imagen completa forma una "mancha".
- Si la mancha se **alarga hacia una dirección**, la imagen tiene mucho de ese color (o una dominante).
- Mancha **muy pegada al centro**: imagen poco saturada (casi blanco y negro, niebla, material Log sin convertir, o un look desaturado intencional). Mancha **llegando a las cajitas o pasándolas**: muy saturada, posible problema para transmisión y piel antinatural.

```text
             R    Mg
        Yl   \   /
              \ /     <- línea de piel entre R y Yl
   G -------- ( ) -------- B
              / \
            Cy
```

### La línea de tono de piel
Es una línea que sale del centro entre el rojo y el amarillo (aproximadamente hacia las 11 en un reloj). **Todos los tonos de piel humanos**, de claros a oscuros, tienen un matiz muy parecido; lo que cambia es la luminosidad y la saturación. Por eso la piel bien corregida cae **sobre o muy cerca de esa línea**. Si cae hacia el magenta, la persona se ve rosada o enferma; si cae hacia el verde/amarillo, se ve verdosa.

En Resolve se activa en la configuración del vectorscopio: **Show Skin Tone Indicator**.

### Ejemplo
Plano medio de una entrevista con el iPhone 15 Pro bajo un foco LED barato:
1. En la página Color, dibuja una ventana circular sobre la cara (o usa el **Qualifier** para seleccionar la piel) y observa el vectorscopio: solo ves la mancha de la piel.
2. La mancha está unos grados hacia el magenta respecto a la línea de piel.
3. En la rueda **Gamma**, empuja ligeramente hacia verde/amarillo hasta que la mancha quede sobre la línea, o ajusta *Tint*.
4. Revisa la saturación: si la mancha se alarga mucho, baja saturación en ese nodo.

### Cuándo usarlo
- Al **corregir** piel y saturación ([[Igualar tomas y tonos de piel]]).
- Al aplicar looks, para ver si alguno empuja la piel fuera de su línea ([[Corrección de color vs gradación]]).
- Para detectar dominantes en conjunto con el [[Parade RGB]].

### Errores comunes
- **Forzar toda la imagen a la línea de piel**: solo la piel va ahí; aísla la cara para medir.
- **Usar el zoom del vectorscopio sin darte cuenta**: en 2x la mancha parece más saturada de lo que es.
- **Saturar hasta "que se vea bonito" en tu monitor**: la mancha te avisa cuando te pasas.

### Para profundizar
- [Página Color de DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve/color)
- [Capacitación oficial de DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve/training)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué muestra el vectorscopio?
> El **tono** (ángulo) y la **saturación** (distancia del centro) de los colores de la imagen.

> [!question]- ¿Qué es la línea de tono de piel?
> Una línea guía (entre amarillo y rojo) donde suelen caer los tonos de piel de cualquier etnia; si la piel se aleja, se ve verde o magenta.

> [!question]- ¿Qué significa una mancha muy pegada al centro?
> Imagen poco saturada (como un Log sin convertir).

## Práctica

- [[Lab 5 - Corrección y gradación de Apple Log con scopes]]

