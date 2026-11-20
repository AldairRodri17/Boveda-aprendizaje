---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Capas, máscaras y modos de fusión

Parte de: [[32-Edicion-Foto/05 Capas y composición/_Índice Capas y composición|Capas y composición]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **capa** es como una hoja de acetato transparente apilada sobre otras: cada una contiene una imagen, un texto, un ajuste o un relleno, y la imagen final es la combinación de todas. Una **máscara de capa** es una imagen en escala de grises asociada a una capa que decide qué partes se ven (blanco), cuáles se ocultan (negro) y cuáles son semitransparentes (gris). Los **modos de fusión** definen *cómo* se combinan los píxeles de una capa con los de abajo, más allá de simplemente taparlos.

### Cómo funciona
**Capas**: el orden importa; lo de arriba tapa lo de abajo. Cada capa tiene opacidad, modo de fusión y visibilidad. En GIMP: diálogo de capas (`Ctrl+L`), nueva capa `Shift+Ctrl+N`, duplicar `Shift+Ctrl+D`. Los **grupos** organizan y, en GIMP 3.2, pueden llevar filtros no destructivos que actúan como capas de ajuste.

**Máscaras**: clic derecho en la capa > *Añadir máscara de capa* (blanco = todo visible). Pinta en negro para ocultar, blanco para mostrar, gris para semitransparencia. Puedes usar degradados para fundir dos imágenes. Mantén `Alt` y haz clic en la máscara para verla sola.

**Modos de fusión más usados** (en GIMP se agrupan por familia):
- **Multiplicar** (*Multiply*): multiplica los valores; el blanco no cambia nada y todo lo demás **oscurece**. Sirve para sombras, oscurecer una foto con una copia de sí misma, o poner un dibujo de líneas negras sobre color.
- **Pantalla** (*Screen*): lo opuesto; el negro no cambia nada y todo lo demás **aclara**. Para brillos, destellos, fuegos artificiales sobre fondo negro, o aclarar una foto oscura.
- **Superponer** (*Overlay*): multiplica las zonas oscuras y aplica pantalla a las claras, así que **aumenta el contraste**; el gris 50 % es neutro. Base de dodge and burn y de texturas.
- **Luz suave** (*Soft light*): como Superponer pero más suave; muy usado en dodge and burn.
- **Color**, **Tono**, **Saturación**, **Luminosidad**: para colorear o separar brillo y color.
- **Extraer/Combinar granulado**: usados en la separación de frecuencias.

### Ejemplo
Foto de la a6700 de un atardecer donde el primer plano quedó oscuro y el cielo bien:
1. Tienes dos TIFF del mismo encuadre revelados distinto (uno para el cielo, otro +2 EV para el suelo). Ábrelos como capas (`Archivo > Abrir como capas`).
2. Pon la versión clara arriba, añade máscara negra (todo oculto) y pinta en blanco el suelo con pincel suave, o usa un degradado vertical en la máscara.
3. Para dar contraste, capa gris 50 % en **Superponer** y pinta luces y sombras suaves.

### Cuándo usarlo y cuándo no
- **Capas y máscaras**: retoque, fotomontaje, combinar exposiciones manualmente, texto y logos.
- **Modos de fusión**: efectos de luz, texturas, oscurecer o aclarar rápido, dodge and burn.
- **No hace falta** para ajustes globales que el revelador hace mejor sobre el RAW.

### Errores comunes
1. **Borrar con la goma en vez de usar máscara**: pierdes los píxeles para siempre. Con la máscara puedes recuperar lo ocultado pintando en blanco, ajustar el borde después o cambiar de idea.
2. **Pintar en la capa y no en la máscara** (fíjate en qué miniatura está seleccionada, tiene borde blanco).
3. **Guardar en JPEG** y perder las capas: guarda XCF.
4. **Demasiadas capas sin nombre**: nómbralas y agrúpalas.

### Para profundizar
- [GIMP 3.0: añadir máscaras de capa](https://docs.gimp.org/3.0/en/gimp-layer-mask-add.html)
- [GIMP 3.2: combinar imágenes (capas)](https://docs.gimp.org/3.2/en/gimp-image-combining.html)
- [GIMP 3.0: modos de capa heredados](https://docs.gimp.org/3.0/en_GB/gimp-concepts-layer-modes-legacy.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una capa?
> Una imagen apilada sobre otras; puedes moverla, cambiar su opacidad o mezclarla sin afectar las demás.

> [!question]- ¿Para qué sirve una máscara de capa?
> Para ocultar partes de la capa sin borrarlas: blanco muestra, negro oculta, gris semitransparente. Siempre puedes corregir.

> [!question]- ¿Qué hacen los modos Multiplicar, Pantalla y Superponer?
> **Multiplicar** oscurece (el blanco desaparece). **Pantalla** aclara (el negro desaparece). **Superponer** aumenta contraste: aclara claros y oscurece oscuros.

> [!question]- Error común: borrar con la goma en vez de usar máscara. ¿Qué pierdes?
> La posibilidad de recuperar lo borrado; con máscara pintas de nuevo en blanco y regresa.

## Práctica

- [[Práctica Edición 07 - Quitar fondo y fotomontaje]]

