---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Selecciones y quitar fondos

Parte de: [[32-Edicion-Foto/05 Capas y composición/_Índice Capas y composición|Capas y composición]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **selección** delimita la zona de la imagen sobre la que actuará la siguiente operación: borrar, copiar, ajustar o crear una máscara. **Quitar el fondo** es seleccionar el sujeto (persona, producto, mascota) y ocultar todo lo demás, para ponerle un fondo blanco, transparente u otra foto. La calidad de un recorte depende casi por completo de lo bien hecha que esté la selección en los bordes.

### Cómo funciona: herramientas de selección en GIMP
- **Rectangular** y **elíptica** (`R`, `E`): formas geométricas.
- **Libre / lazo** (`F`): a mano alzada o por puntos.
- **Varita mágica / selección difusa** (`U`): selecciona píxeles contiguos de color parecido, con *umbral*.
- **Por color** (`Shift+O`): todos los píxeles de un color, aunque no sean contiguos (fondos lisos).
- **Tijeras inteligentes** (`I`): siguen bordes con contraste.
- **Selección de primer plano** (*Foreground select*): trazas un contorno aproximado, pintas el sujeto, y GIMP calcula el borde, con buen resultado en pelo.
- **Rutas** (`B`): curvas Bézier precisas, ideales para productos con bordes duros; luego *Selección desde ruta*.
- **Máscara rápida** (`Shift+Q`): ves la selección como una capa roja y la pintas con pinceles: blanco agrega, negro quita.
- Refinar: `Seleccionar > Difuminar`, *Encoger/Agrandar*, *Eliminar agujeros*. Lo mejor: convierte la selección en **máscara de capa** para corregir pintando.

En **Photopea**, `Seleccionar > Sujeto` usa IA y *Refinar borde* ayuda con pelo. En **darktable 5.6**, la máscara de objeto por IA sirve para ajustes locales, no para recortar a transparente.

### En el iPhone 15 Pro
La app **Fotos** permite **separar el sujeto del fondo**: abre la foto, **mantén presionado el sujeto** hasta que aparezca un contorno brillante y elige *Copiar* o *Compartir* (o arrástralo a otra app). El resultado es un PNG con fondo transparente. Funciona con personas, mascotas y objetos bien definidos. Es rápido para stickers, presentaciones o un catálogo informal; para un trabajo fino, pásalo a GIMP y corrige la máscara.

### Ejemplo
Foto de producto con la a6700 (una taza sobre fondo gris) para la tienda en línea de un amigo:
1. En GIMP, *Rutas* alrededor del contorno (bordes duros), *Selección desde ruta*, `Seleccionar > Difuminar` 1 px.
2. *Añadir máscara de capa* > *Selección*.
3. Capa nueva debajo con blanco puro y una sombra suave en Multiplicar.
4. Exporta PNG o JPEG según la tienda.

### La zona más difícil: pelo y bordes semitransparentes
Pelo, pelaje, velos, humo y vidrio no tienen un borde sólido: cada mechón mezcla el color del sujeto con el del fondo. Cómo tratarlo:
- Usa *Selección de primer plano* o la selección de sujeto, y luego **refina la máscara** pintando con un pincel suave a baja opacidad.
- Técnica por canales: duplica el canal con más contraste entre pelo y fondo (a menudo el azul), súbele el contraste con *Niveles* o *Curvas* hasta que el pelo quede negro y el fondo blanco, y úsalo como máscara.
- **Descontaminar el color**: al poner un fondo nuevo, los bordes conservan el color viejo (halo verde de una pared, por ejemplo). Pinta encima con modo *Color* tomando color del pelo, o desatura el borde.
- **Al disparar**: fondo liso, separado del sujeto y con color distinto al pelo facilita muchísimo el recorte.

### Cuándo usarlo y cuándo no
- **Úsalo** para producto, fotomontajes, miniaturas, stickers.
- **No lo hagas a mano** si solo necesitas desenfocar o oscurecer el fondo: un ajuste local con máscara difuminada en el revelador es más rápido y natural.

### Errores comunes
1. **Bordes duros o con "escalones"** por no difuminar.
2. **Halo del fondo original** alrededor del sujeto.
3. **Recortar el pelo como casco**: se ve recortado; deja mechones.
4. **Luz que no coincide** con el fondo nuevo (ver [[Fotomontaje]]).

### Para profundizar
- [GIMP 3.0: documentación](https://docs.gimp.org/3.0/en/)
- [GIMP 3.0: añadir máscaras de capa](https://docs.gimp.org/3.0/en/gimp-layer-mask-add.html)
- [[Capas, máscaras y modos de fusión]] y [[Fotografía de producto y comida]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué herramientas de selección usas en GIMP?
> Rectangular/elíptica, **Selección difusa** (varita), **Selección por color**, **Tijeras inteligentes**, **Selección de primer plano** y Rutas para bordes precisos.

> [!question]- ¿Cómo quitas el fondo de un sujeto en el iPhone?
> En Fotos, mantén presionado el sujeto y elige **Copiar** o compártelo: iOS lo separa del fondo. Útil para pruebas rápidas.

> [!question]- ¿Qué zona es la más difícil y cómo la tratas?
> El **cabello** y bordes finos. Usa selección de primer plano o refina la máscara con un pincel suave; en fondos lisos funciona mejor.

## Práctica

- [[Práctica Edición 07 - Quitar fondo y fotomontaje]]

