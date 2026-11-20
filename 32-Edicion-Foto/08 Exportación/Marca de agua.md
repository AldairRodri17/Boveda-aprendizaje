---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Marca de agua

Parte de: [[32-Edicion-Foto/08 Exportación/_Índice Exportación|Exportación]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **marca de agua** es un texto o logotipo (tu nombre, tu usuario, tu marca) que se superpone a la foto exportada para identificar al autor. Es una forma de firma visible. No es lo mismo que los **metadatos** de autor y derechos (EXIF/IPTC), que van dentro del archivo y no se ven en la imagen (ver [[Metadatos EXIF e IPTC]]).

### ¿Sirve para proteger tus fotos?
**Solo un poco.** Una marca discreta en una esquina se quita en segundos recortando o con herramientas de IA para borrar objetos. Una marca grande en el centro sí dificulta el uso, pero arruina la foto para quien la quiere disfrutar. Lo que realmente te protege es que **tus derechos de autor existen desde que tomas la foto**, que guardas los RAW originales como prueba, y que pones tu nombre en los metadatos. La marca de agua funciona más como **firma y promoción**: que quien vea tu foto compartida sepa de quién es y te encuentre. Para clientes que revisan pruebas antes de pagar, una marca grande y semitransparente sí tiene sentido.

### Cómo colocarla para que no estorbe
- **Pequeña**: alrededor de 3–5 % del ancho de la imagen.
- **Esquina o borde inferior**, en una zona sin detalle importante; respeta un margen.
- **Semitransparente**: opacidad del 30–60 %, en blanco o negro según el fondo, sin sombra pesada.
- **Tipografía simple** y legible; un logo en PNG transparente o SVG.
- **Consistente** en toda tu obra: misma posición y tamaño relativo.
- En historias de Instagram, fuera de las zonas que tapa la interfaz.

### Cómo agregarla automáticamente en darktable
darktable tiene el módulo de procesamiento **watermark** (marca de agua), que usa un archivo **SVG** como plantilla:
1. Crea un SVG con tu texto o logo (o usa los de ejemplo de darktable). Los SVG admiten **variables** que darktable rellena por foto, como la fecha, los datos de la cámara o el autor guardado en los metadatos (Pendiente de confirmar la lista exacta de variables en el manual).
2. Copia el SVG a la carpeta de marcas de agua de tu configuración de darktable (en Windows, dentro de `%LOCALAPPDATA%\darktable\watermarks`).
3. En el cuarto oscuro activa *watermark*, elige el SVG, la **escala** (relativa a la imagen, por ejemplo 4 % del lado mayor), la **alineación** (abajo a la derecha), el desplazamiento y la **opacidad**.
4. Guarda **solo ese módulo** como **estilo** ("Marca de agua").
5. En el módulo de **exportación**, en el campo *estilo*, elige "Marca de agua". darktable la aplica al exportar sin tocar tu edición: las fotos en la biblioteca quedan limpias y solo las exportadas llevan marca.

Al mismo tiempo, en el módulo de metadatos de la mesa de luz llena *creador* y *derechos* para que vayan en el EXIF/XMP del JPEG.

En **GIMP**: capa de texto o logo en PNG, opacidad al 50 %, posición en esquina; guarda como plantilla XCF. En el iPhone, apps de terceros o un atajo de Shortcuts pueden hacerlo, pero desde la PC es más controlable.

### Ejemplo
Exportas 30 fotos de un evento con la a6700 para el cliente: preset de exportación "Cliente web" con estilo "Marca de agua grande" (centrada, 25 % de opacidad) para las pruebas, y otro preset "Entrega final" sin marca y en alta resolución, que mandas cuando confirma.

### Cuándo usarlo y cuándo no
- **Úsala** en pruebas para clientes, fotos que circulan mucho y quieres que te identifiquen, y portafolio en redes si te gusta.
- **Evítala** en concursos (casi todos la prohíben), en entregas finales pagadas, y si es tan grande que distrae.

### Errores comunes
1. **Marca enorme y opaca** en tu portafolio: aleja a quien mira.
2. **Pensar que te protege legalmente** más que los metadatos y los RAW.
3. **Aplicarla en la edición** en vez de en la exportación: queda en todas las versiones.
4. **Tamaño fijo en píxeles**: en una foto pequeña se ve gigante; usa escala relativa.

### Para profundizar
- [darktable 5.6: módulos de procesamiento (watermark)](https://docs.darktable.org/usermanual/5.6/en/module-reference/processing-modules/)
- [darktable 5.6: módulo de exportación (estilo al exportar)](https://docs.darktable.org/usermanual/5.6/en/module-reference/utility-modules/shared/export/)
- [[Derechos de autor de fotos y videos]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Sirve una marca de agua para proteger tus fotos?
> Disuade usos casuales y da crédito, pero se puede recortar o quitar. No sustituye tus derechos de autor.

> [!question]- ¿Dónde y cómo la colocas para que no estorbe?
> Pequeña, semitransparente, en una esquina o borde donde no compita con el sujeto.

> [!question]- ¿Cómo la agregas automáticamente en darktable?
> Con el módulo **watermark** (marca de agua) usando un SVG; guárdalo en un estilo de exportación para aplicarlo a todas.

## Práctica

- [[Práctica Edición 11 - Exportar para Instagram e impresión]]

