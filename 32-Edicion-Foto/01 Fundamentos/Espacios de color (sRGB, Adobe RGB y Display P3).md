---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Espacios de color (sRGB, Adobe RGB y Display P3)

Parte de: [[32-Edicion-Foto/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **espacio de color** define qué colores exactos significan los números RGB de un archivo. El mismo valor `R=255, G=0, B=0` es un rojo distinto en sRGB que en Display P3, porque cada espacio tiene sus propios *primarios* (qué tan puro es su rojo, verde y azul), su punto blanco y su curva de gamma. El conjunto de colores que abarca se llama **gama** (*gamut*). Sin saber el espacio, el programa no sabe cómo mostrar la foto.

### Cómo funciona
De menor a mayor gama, los tres que más vas a ver:

| Espacio | Gama | Dónde lo encuentras |
|---|---|---|
| **sRGB** | La más pequeña | Estándar de la web, la mayoría de monitores y redes sociales |
| **Display P3** | ~25 % más amplio que sRGB, sobre todo en rojos y verdes | Pantallas de iPhone, Mac y monitores modernos; las fotos del iPhone 15 Pro se guardan en P3 |
| **Adobe RGB (1998)** | Similar en área a P3, más extendido en verdes y cianes | Impresión, monitores para foto, opción en la a6700 para JPEG |

Así que, de más chico a más amplio: **sRGB < Display P3 ≈ Adobe RGB** (P3 llega más lejos en rojos, Adobe RGB en verdes-cianes). Más grandes aún existen ProPhoto RGB y Rec. 2020, usados como espacios de trabajo.

La información del espacio viaja en el archivo como **perfil ICC incrustado**. El sistema de gestión de color convierte del perfil del archivo al perfil de tu monitor (ver [[Calibración del monitor]]).

En los RAW no hay espacio elegido aún: el revelador trabaja internamente en un espacio amplio (darktable usa Rec. 2020 lineal como espacio de trabajo) y tú eliges el espacio solo al exportar.

### Ejemplo
- **Instagram/web**: exporta en darktable con *perfil de salida: sRGB*. Todos los teléfonos y navegadores lo muestran igual.
- **Fotos del iPhone 15 Pro**: vienen en Display P3. Si las subes desde el teléfono, Instagram las convierte; si las editas en la PC para web, conviértelas a sRGB al exportar.
- **Imprimir en laboratorio**: pregunta qué aceptan. Muchos piden sRGB; los de gama alta aceptan Adobe RGB y te dan su perfil ICC.
- **Archivo intermedio a GIMP**: TIFF de 16 bits en Adobe RGB o ProPhoto, para no recortar colores antes del retoque.

### Cuándo usarlo y cuándo no
- **sRGB**: siempre para web, redes, WhatsApp, correo. Es lo seguro.
- **Display P3**: si sabes que se verá en pantallas Apple o navegadores con gestión de color y quieres colores más intensos (atardeceres, flores).
- **Adobe RGB**: impresión profesional y archivos intermedios. No lo uses para publicar.
- En la a6700, si solo disparas RAW, el ajuste de espacio de color de la cámara solo afecta al JPEG y a la vista previa, no al RAW.

### Errores comunes
1. **Exportar en Adobe RGB a Instagram**: la app o el navegador puede ignorar o mal convertir el perfil y la foto se ve **deslavada, con colores apagados**, porque los números pensados para una gama amplia se interpretan como sRGB.
2. **Quitar el perfil ICC al exportar** para ahorrar bytes: sin perfil, cada programa supone algo distinto.
3. **Convertir varias veces** entre espacios: cada conversión puede recortar colores fuera de gama. Convierte una sola vez, al exportar.
4. **Creer que un espacio amplio se ve "mejor" en cualquier pantalla**: si tu monitor solo cubre sRGB, no verás la diferencia y no podrás juzgar esos colores.

### Para profundizar
- [Introducción a los perfiles ICC (International Color Consortium)](https://www.color.org/getting-started/)
- [Preguntas frecuentes del ICC](https://www.color.org/faqs/)
- [darktable 5.6: exportación y perfil de salida](https://docs.darktable.org/usermanual/5.6/en/module-reference/utility-modules/shared/export/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un espacio de color?
> El conjunto de colores que se puede representar con un sistema de números RGB. Espacios más amplios (gamut) incluyen colores más saturados.

> [!question]- ¿Cuál es más amplio: sRGB, Display P3 o Adobe RGB?
> sRGB es el más pequeño. Display P3 y Adobe RGB son más amplios: P3 se extiende más en rojos y amarillos, Adobe RGB en verdes y cianes.

> [!question]- ¿Qué espacio usas al exportar para web y redes?
> **sRGB**: es el estándar que muestran correctamente casi todos los navegadores y pantallas. Las fotos del iPhone vienen en Display P3; conviértelas al exportar.

> [!question]- Error común: exportar en Adobe RGB a Instagram. ¿Qué pasa?
> Si la app no gestiona color, los colores se ven deslavados o raros. Exporta sRGB para web.

## Práctica

- [[Práctica Edición 01 - Prepara tu estación de edición]]

