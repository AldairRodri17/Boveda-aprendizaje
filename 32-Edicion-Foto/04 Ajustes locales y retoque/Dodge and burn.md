---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Dodge and burn

Parte de: [[32-Edicion-Foto/04 Ajustes locales y retoque/_Índice Ajustes locales y retoque|Ajustes locales y retoque]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Dodge and burn** (aclarar y quemar) es aclarar y oscurecer zonas concretas de la foto para modelar la luz. El nombre viene del cuarto oscuro: el fotógrafo tapaba parte del papel bajo la ampliadora (*dodge*, menos luz, queda más claro) o le daba más exposición a otra zona (*burn*, más luz, queda más oscuro). Hoy sirve para dar volumen a un rostro, resaltar el sujeto, guiar la mirada y emparejar zonas disparejas.

### Cómo funciona
El ojo va primero a lo más claro y con más contraste. Al aclarar lo importante y oscurecer lo secundario, construyes un recorrido visual. Hay dos escalas:
- **Macro** (global-local): aclarar el sujeto completo y oscurecer bordes y fondo. Es casi una viñeta a medida.
- **Micro**: en retrato, suavizar irregularidades aclarando las pequeñas sombras de la piel y oscureciendo brillos; o esculpir pómulos y mandíbula.

**Patrón general que funciona** en retrato y paisaje: **aclara donde ya cae la luz y oscurece donde ya hay sombra**, siguiendo la dirección de la luz original, y oscurece suavemente los bordes del encuadre. Así el volumen aumenta sin que se note la intervención.

### Cómo se hace en GIMP
Método no destructivo con capa gris:
1. Capa nueva (`Shift+Ctrl+N`), relleno **gris 50 %** (`#808080`), modo **Luz suave** (*Soft light*) o **Superponer** (*Overlay*). El gris neutro no cambia nada en esos modos.
2. Pincel suave, **opacidad 5–10 %**, color **blanco** para aclarar (dodge) y **negro** para oscurecer (burn). Alterna con `X`.
3. Pinta en pasadas suaves. Si te pasaste, pinta con gris 50 % para neutralizar.
4. Ajusta la opacidad de toda la capa para dosificar.

GIMP también tiene la herramienta *Blanquear/Quemar* (`Shift+D`), pero actúa directamente sobre los píxeles; úsala sobre una capa duplicada.

### Cómo se hace en darktable
- **Exposición enmascarada**: dos instancias de *exposición*, "dodge" (+0.5 EV) y "burn" (−0.5 EV), cada una con **máscara dibujada a pincel** o con formas difuminadas. Puedes agregar formas a la misma máscara.
- **Ecualizador de tonos**: para aclarar u oscurecer zonas de brillo parecido sin pintar.
- La documentación de darktable 5.6 sugiere precisamente esas dos vías.

### Ejemplo
Retrato con la a6700, luz de ventana a la izquierda:
- Dodge suave en el lado iluminado de la cara, en los ojos (iris y blanco, muy poco) y en el pelo con brillo.
- Burn en el lado en sombra del cuello, en el fondo junto a la cabeza y en las esquinas.
- Resultado: más tridimensional y la mirada va a los ojos.

En paisaje: aclara el camino que lleva a la montaña y quema el primer plano y las esquinas.

### Cuándo usarlo y cuándo no
- **Úsalo** cuando la luz es plana o el sujeto no destaca, y en blanco y negro (es la base de la tradición del cuarto oscuro).
- **No lo uses** como sustituto de una buena exposición, ni en fotoperiodismo si altera la información de la escena (ver [[Ética del retoque]]).

### Errores comunes
1. **Opacidad alta**: manchas visibles. Mejor muchas pasadas suaves.
2. **Ir contra la luz**: aclarar zonas que deberían estar en sombra rompe la credibilidad.
3. **Halos alrededor del sujeto** por un pincel demasiado grande junto al borde.
4. **Cambiar el color** sin querer: en modo Superponer, el burn puede saturar; usa Luz suave o baja la saturación de la zona.

### Para profundizar
- [darktable 5.6: flujo recomendado (dodge and burn)](https://docs.darktable.org/usermanual/5.6/en/overview/workflow/process/)
- [GIMP 3.0: añadir máscaras de capa](https://docs.gimp.org/3.0/en/gimp-layer-mask-add.html)
- [[Máscaras y ajustes locales]] y [[Dirección de la luz]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es el dodge and burn?
> Aclarar (dodge) y oscurecer (burn) zonas específicas para guiar la mirada y modelar el volumen. Viene del cuarto oscuro químico.

> [!question]- ¿Cómo se hace en GIMP?
> Capa nueva rellena de gris 50 % en modo **Superponer** o **Luz suave**; pinta con blanco (aclarar) o negro (oscurecer) a opacidad baja (5–15 %).

> [!question]- ¿Cómo se hace en darktable?
> Con dos instancias del módulo **exposición** (+ y −) con máscaras dibujadas a mano, o con el **ecualizador de tonos** usando máscaras.

> [!question]- ¿Qué patrón general funciona en retrato y paisaje?
> Aclarar sutilmente el sujeto y zonas de interés, y oscurecer bordes y elementos que distraen.

## Práctica

- [[Práctica Edición 05 - Máscaras locales y dodge and burn]]
- [[Práctica Edición 08 - Blanco y negro con intención]]

