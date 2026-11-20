---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Fotomontaje

Parte de: [[32-Edicion-Foto/05 Capas y composición/_Índice Capas y composición|Capas y composición]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **fotomontaje** combina partes de dos o más fotos para crear una imagen nueva: un sujeto en otro fondo, un cielo de otro día, una escena imposible. Puede ser realista (que nadie note la unión) o claramente surrealista. Técnicamente se apoya en todo lo anterior: selecciones, máscaras, capas, modos de fusión y ajustes de color.

### Qué lo hace creíble
El ojo detecta inconsistencias aunque no sepa explicarlas. Para que un montaje funcione, todos los elementos deben compartir:
1. **Dirección y dureza de la luz**: si en el fondo el sol viene de la izquierda y es duro, el sujeto debe estar iluminado igual. Esto se decide **al disparar**: no hay edición que invierta una luz.
2. **Perspectiva y altura de cámara**: la línea del horizonte del fondo debe coincidir con la altura de la cámara con la que fotografiaste al sujeto. Una persona fotografiada desde arriba no encaja en un fondo tomado a ras de suelo.
3. **Escala y profundidad de campo**: tamaños coherentes; si el fondo está desenfocado, el sujeto nítido debe parecer estar a la distancia correcta.
4. **Sombras de contacto**: una sombra suave bajo los pies o el objeto lo "asienta" en el suelo.
5. **Color, contraste y ruido**: mismo balance de blancos, mismos negros y mismo grano.

### Cómo igualar color entre sujeto y fondo
- **Negros y blancos**: con *Niveles* o *Curvas* en el sujeto (como capa recortada a él), iguala el punto más oscuro y el más claro a los del fondo. Mira por canal: si las sombras del fondo son azuladas, las del sujeto también deben serlo.
- **Tono general**: una capa de color tomado del fondo, en modo *Color* o *Luz suave* a baja opacidad sobre el sujeto, lo baña del mismo ambiente.
- **Saturación** parecida.
- **Truco de verificación**: agrega temporalmente una capa negra en modo *Color* (todo en gris) para comparar brillos sin que el color te engañe; luego otra en modo *Luminosidad* para comparar solo color.
- **Grano**: si el fondo tiene ruido (ISO alto), añade ruido parecido al sujeto.
- **Borde**: un poco de "luz envolvente" (pintar con el color del fondo en Pantalla a muy baja opacidad en el contorno) ayuda a integrar.

### Ejemplo
Quieres poner a tu primo, fotografiado con la a6700 en el patio, frente a un paisaje que tomaste con el DJI Mini 4K al atardecer:
1. Antes de disparar, revisa en qué dirección y altura venía el sol en la foto del dron, y fotografía al primo con la luz desde ese lado y con la cámara a una altura coherente.
2. Revela ambas en darktable con el mismo balance de blancos aproximado y exporta TIFF 16 bits.
3. En GIMP: fondo abajo, sujeto arriba con máscara (ver [[Selecciones y quitar fondos]]).
4. Igualación de color con Curvas por canal y una capa de color naranja en Luz suave al 20 %.
5. Sombra de contacto: capa en Multiplicar con pincel negro suave al 30 %.
6. Ruido uniforme y una curva general al final para unir todo.

### Consideraciones éticas y legales
- **Transparencia**: si el montaje puede pasar por real y se usa para informar, opinar o vender, dilo. Presentar un montaje como foto documental es engaño, y en concursos documentales está prohibido (ver [[Ética del retoque]]).
- **Derechos de autor**: solo usa fotos tuyas, con licencia que lo permita o con permiso. Bajar un fondo de internet no te da derecho a usarlo (ver [[Derechos de autor de fotos y videos]]).
- **Imagen de las personas**: no pongas a alguien en una situación que no vivió, sobre todo si puede afectar su reputación, sin su consentimiento.

### Cuándo usarlo y cuándo no
- **Úsalo** en arte, publicidad, carteles, portadas, humor, cambios de cielo bien declarados.
- **No** en fotos que se presentan como evidencia de algo que pasó.

### Errores comunes
1. Luz incoherente.
2. Bordes duros o con halo.
3. Falta de sombras de contacto: el sujeto "flota".
4. Diferente nitidez y grano entre capas.

### Para profundizar
- [GIMP 3.2: combinar imágenes](https://docs.gimp.org/3.2/en/gimp-image-combining.html)
- [World Press Photo: qué cuenta como manipulación](https://www.worldpressphoto.org/contest/verification-process/what-counts-as-manipulation)
- [[Capas, máscaras y modos de fusión]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace creíble un fotomontaje?
> Que coincidan la **dirección y dureza de la luz**, la **perspectiva**, el **color** y la **nitidez/ruido** entre los elementos.

> [!question]- ¿Cómo igualas color entre el sujeto y el nuevo fondo?
> Con capas de ajuste o curvas recortadas al sujeto, y revisando en blanco y negro que las luminosidades coincidan.

> [!question]- ¿Qué consideración ética y legal tiene?
> Usa imágenes propias o con licencia, y no presentes un montaje como foto real si puede engañar (ver [[Ética del retoque]]).

## Práctica

- [[Práctica Edición 07 - Quitar fondo y fotomontaje]]

