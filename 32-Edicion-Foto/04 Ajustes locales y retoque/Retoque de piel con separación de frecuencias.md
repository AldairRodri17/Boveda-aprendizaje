---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Retoque de piel con separación de frecuencias

Parte de: [[32-Edicion-Foto/04 Ajustes locales y retoque/_Índice Ajustes locales y retoque|Ajustes locales y retoque]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **separación de frecuencias** es una técnica de retoque que divide la foto en dos capas: una de **baja frecuencia**, con el color y el tono (las transiciones suaves de luz y las manchas de color), y otra de **alta frecuencia**, con la **textura** (poros, vellos, líneas finas). Al separarlas puedes emparejar manchas de color y tono sin tocar los poros, o limpiar imperfecciones en la textura sin alterar el color. Bien hecha, la piel se ve uniforme pero real.

### Cómo funciona
"Frecuencia" se refiere a qué tan rápido cambia el brillo en el espacio. Un desenfoque gaussiano conserva solo lo que cambia lentamente (baja frecuencia). Si restas esa versión borrosa de la original, lo que queda es el detalle fino (alta frecuencia). Al recombinarlas con el modo de fusión adecuado, obtienes exactamente la imagen original; así que puedes editar cada parte por separado.

El **radio** del desenfoque decide qué se considera textura: debe ser justo el necesario para que en la capa baja **desaparezcan los poros**. Depende de la resolución y de qué tan grande es la cara en el encuadre: en un retrato de medio cuerpo con la a6700 (26 MP) suele andar entre 4 y 10 px.

### Cómo hacerla en GIMP
1. Abre el retrato (ya revelado y exportado como TIFF de 16 bits). Duplica la capa **dos veces**. Nombra la de abajo "baja" y la de arriba "alta".
2. Oculta "alta". En "baja": `Filtros > Desenfocar > Desenfoque gaussiano` con el radio en que dejas de ver poros.
3. Muestra "alta" y cámbiale el modo a **Extraer granulado** (*Grain extract*). Con "alta" seleccionada, clic derecho > **Nuevo desde lo visible**. Esa nueva capa es tu alta frecuencia; borra la "alta" anterior.
4. Pon la nueva capa en modo **Combinar granulado** (*Grain merge*) encima de "baja". La imagen debe verse idéntica a la original.
5. **Textura** (capa alta): *Corregir* o *Clonar* con muestreo solo de esa capa para quitar granitos y pelitos sin tocar el color.
6. **Color y tono** (capa baja): pincel suave a baja opacidad tomando color de la piel vecina con `Ctrl`, o selecciona una zona con bordes muy difuminados y aplica un desenfoque extra para emparejar manchas.
7. Agrupa ambas capas y baja la opacidad del grupo, o enmascara para que el efecto solo afecte piel y no ojos, cejas, labios ni pelo.

Alternativa: `Filtros > Realzar > Descomposición wavelet` (*Wavelet decompose*) separa en varias escalas a la vez; es útil para retocar por niveles de detalle. En darktable, el módulo *retoque* tiene modo ondículas para algo parecido.

### Ejemplo
Retrato de una amiga con luz dura del mediodía, manchas rojizas en mejillas y algunos granitos: en la capa baja emparejas el enrojecimiento con pinceladas de 10 % de opacidad; en la alta corriges los granitos. Al final, el grupo al 70 % de opacidad y un poco de dodge and burn (ver [[Dodge and burn]]).

### Cuándo usarlo y cuándo no
- **Úsala** en retrato y belleza, para tono desigual o manchas temporales, y en producto con superficies que deben verse uniformes.
- **No la uses** cuando con corregir algunos granitos basta (lo más común), ni para alterar rasgos permanentes sin permiso (ver [[Ética del retoque]]).

### Errores comunes
1. **Piel de plástico**: la causa es desenfocar o pintar de más **en la capa de alta frecuencia** (se pierde la textura), usar un radio enorme, o aplicar el efecto al 100 % sobre toda la cara. La piel real tiene poros y pequeñas variaciones de tono.
2. **Radio mal elegido**: muy pequeño, la textura queda en la baja; muy grande, aparecen halos en bordes.
3. **Modos de fusión equivocados** (Extraer/Combinar granulado) o trabajar en 8 bits: la recombinación no queda exacta.
4. **No enmascarar ojos, cejas, labios y pelo**: se ven borrosos.

### Para profundizar
- [GIMP 3.0: Descomposición wavelet](https://docs.gimp.org/3.0/en/plug-in-wavelet-decompose.html)
- [GIMP 3.0: herramienta Corregir](https://docs.gimp.org/3.0/en/gimp-tool-heal.html)
- [GIMP 3.0: añadir máscaras de capa](https://docs.gimp.org/3.0/en/gimp-layer-mask-add.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es la separación de frecuencias?
> Dividir la imagen en una capa de **baja frecuencia** (color y tono) y otra de **alta frecuencia** (textura), para corregir manchas de tono sin borrar la textura de la piel.

> [!question]- ¿Cómo la haces en GIMP?
> Filtros → Mejorar → **Descomposición wavelet** (automático), o a mano: duplica la capa dos veces, desenfoca la copia de abajo (baja frecuencia), pon la de arriba en modo **Extracción de granulado**, crea **Capa → Nueva desde lo visible** y pon esa capa nueva en modo **Combinación de granulado** (es la alta frecuencia).

> [!question]- Error común: piel de plástico. ¿Qué la causa?
> Desenfocar o borrar la capa de textura, o suavizar demasiado la baja frecuencia. Corrige manchas puntuales y deja poros y textura.

## Práctica

- [[Práctica Edición 06 - Limpieza y retoque de piel]]

