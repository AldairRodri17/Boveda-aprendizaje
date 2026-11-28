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
# Formatos de exportación de imagen (JPEG, PNG, TIFF, HEIC, WebP y AVIF)

Parte de: [[32-Edicion-Foto/08 Exportación/_Índice Exportación|Exportación]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **formato de exportación** es el tipo de archivo en que guardas la versión final (o intermedia) de tu foto. Cada formato decide cómo se comprime la imagen, cuántos bits por canal guarda, si admite transparencia, capas o perfiles de color, y qué tan compatible es. Elegir bien evita pérdida de calidad, archivos enormes o fotos que no se abren en otro equipo.

### Cómo funciona
| Formato | Compresión | Bits | Transparencia | Uso típico |
|---|---|---|---|---|
| **JPEG** | Con pérdida | 8 | No | Compartir, web, redes, laboratorio; compatible con todo |
| **PNG** | Sin pérdida | 8 o 16 | Sí | Gráficos, capturas, logos, recortes con fondo transparente |
| **TIFF** | Sin pérdida (o sin compresión) | 8, 16 o 32 flotante | Sí; capas en algunos programas | Archivo intermedio entre revelador y editor, impresión, archivo maestro |
| **HEIF/HEIC** | Con pérdida (HEVC), o sin pérdida | 8 o 10+ | Sí | Formato por defecto del iPhone 15 Pro; ocupa aproximadamente la mitad que un JPEG parecido |
| **WebP** | Con o sin pérdida | 8 | Sí | Web; más ligero que JPEG |
| **AVIF** | Con o sin pérdida (AV1) | 8, 10 o 12 | Sí | Web moderna; muy eficiente, admite HDR y gama amplia |

- **JPEG**: elige calidad 85–95 para publicar. Es el más compatible.
- **PNG y TIFF**: para no perder nada. Un TIFF de 16 bits de la a6700 (6192 × 4128) pesa unos 150 MB sin comprimir; úsalo como intermedio hacia GIMP.
- **HEIF/HEIC**: HEIF es el contenedor; HEIC es HEIF con compresión HEVC. Conviértelo a JPEG cuando lo vas a mandar a alguien con Windows sin las extensiones instaladas, a un laboratorio, a una web que no lo acepte o a un programa viejo. darktable 5.6 ya **exporta** HEIF (con pérdida o sin pérdida, 8, 10 o 12 bits).
- **WebP y AVIF**: su ventaja es menor tamaño con la misma calidad visual que JPEG, transparencia, y en AVIF más bits y HDR. Los navegadores modernos los soportan; ideales para tu portafolio web. Algunas redes los recomprimen o no los aceptan para subir; para redes, JPEG sigue siendo lo seguro.

### Ejemplo
De un ARW de la a6700 revelado en darktable:
- `retrato_master.tif`: TIFF 16 bits Adobe RGB, para retocar en GIMP y archivar.
- `retrato_ig.jpg`: JPEG sRGB calidad 90, 1080 × 1350, para Instagram.
- `retrato_web.avif`: AVIF sRGB 2000 px de lado largo, para tu sitio (en el HTML puedes ofrecer AVIF con JPEG de respaldo).
- `logo.png`: tu marca de agua con fondo transparente.

Del iPhone 15 Pro: si compartes el HEIC a tu PC con Windows, convierte a JPEG desde el propio iPhone (*Configuración > Fotos > Transferir a Mac o PC: Automático*) o exporta desde darktable.

### Cuándo usarlo y cuándo no
- **JPEG**: entregar y publicar. **No** para editar y guardar una y otra vez.
- **TIFF/PNG 16 bits**: intermedios y archivo. **No** para web (enormes).
- **HEIF**: archivo eficiente en el ecosistema Apple. **No** para enviar a quien no lo puede abrir.
- **WebP/AVIF**: web propia. **No** cuando el destino no lo soporta.

### Errores comunes
1. **Guardar un JPEG una y otra vez**: cada vez que lo abres, editas y guardas, vuelve a comprimirse y pierde calidad (**pérdida generacional**): aparecen bloques de 8 × 8 px, halos en bordes y bandas en cielos. Edita desde el RAW o un TIFF y exporta el JPEG una sola vez.
2. **Exportar sin perfil ICC** o en un espacio que no es sRGB para web.
3. **JPEG calidad 100**: archivo mucho más pesado con mejora casi invisible frente a 92–95.
4. **Usar PNG para fotos en web**: pesa varias veces más que un JPEG o AVIF.

### Para profundizar
- [MDN: guía de tipos y formatos de imagen](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats/Image_types)
- [darktable 5.6: módulo de exportación](https://docs.darktable.org/usermanual/5.6/en/module-reference/utility-modules/shared/export/)
- [[RAW vs JPEG y HEIF]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuándo usas JPEG, PNG y TIFF?
> **JPEG**: fotos para compartir (con pérdida, 8 bits). **PNG**: gráficos, texto, transparencias (sin pérdida, pesado para fotos). **TIFF 16 bits**: archivo intermedio para retocar o imprimir.

> [!question]- ¿Qué ventajas tienen WebP y AVIF?
> Comprimen mejor que JPEG con calidad parecida, ideales para web; no todos los programas ni servicios los aceptan.

> [!question]- ¿Qué es HEIC/HEIF y cuándo lo conviertes?
> El formato de foto del iPhone: alta calidad en poco espacio. Conviértelo a JPEG para compartir con quien use programas que no lo abren.

> [!question]- Error común: guardar un JPEG una y otra vez. ¿Qué pasa?
> Cada vez se recomprime y se acumulan artefactos. Edita desde el original y exporta una sola vez.

## Práctica

- [[Práctica Edición 11 - Exportar para Instagram e impresión]]

