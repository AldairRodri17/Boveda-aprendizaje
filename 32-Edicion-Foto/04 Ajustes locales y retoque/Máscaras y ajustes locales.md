---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Edición de fotografía
tags: [edicion-foto]
---
# Máscaras y ajustes locales

Parte de: [[32-Edicion-Foto/04 Ajustes locales y retoque/_Índice Ajustes locales y retoque|Ajustes locales y retoque]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **ajuste local** es cualquier cambio que se aplica solo a una parte de la foto: oscurecer el cielo, aclarar una cara, quitar saturación a un letrero que distrae. Para decirle al programa dónde aplicar el cambio se usa una **máscara**: una imagen en escala de grises donde el blanco significa "aplica al 100 %", el negro "no apliques" y los grises "aplica parcialmente". Los ajustes globales corrigen la foto; los locales dirigen la mirada.

### Cómo funciona
En darktable casi cualquier módulo de procesamiento acepta máscara desde su pie (*fusión*, el ícono de círculo). Tipos de máscara:
- **Dibujada** (*drawn*): círculo, elipse, ruta (polígono con curvas), pincel y **degradado** (*gradient*). Cada forma tiene **difuminado** (*feather*) para que el borde sea suave.
- **Paramétrica**: selecciona por valores del píxel (luminosidad, croma, tono, o canales RGB). Por ejemplo, "solo píxeles muy claros" o "solo tonos azules", con rampas suaves en los extremos.
- **Dibujada y paramétrica combinadas**: una forma limita la zona y los parámetros refinan dentro de ella (el degradado más "solo lo claro" = cielo sin edificios).
- **Raster**: reutilizas la máscara de otro módulo.
- **Máscara de objeto por IA** (5.6, opcional): clic sobre un objeto y el modelo SAM2.1 o SegNext genera la máscara.
- **Refinamiento**: desenfoque de la máscara, *filtro guiado* que hace que la máscara siga los bordes reales de la imagen, opacidad y modos de fusión.

En Lightroom móvil las máscaras son Premium; en Snapseed (gratis) usa *Selectivo* y el *Pincel de pila*; en GIMP, máscaras de capa (ver [[Capas, máscaras y modos de fusión]]).

### Ejemplo: oscurecer solo el cielo con degradado
Paisaje de la a6700 con cielo brillante y montañas:
1. Agrega una **nueva instancia** del módulo *exposición* (menú de instancias > nueva instancia) para no tocar la exposición global; nómbrala "cielo".
2. Exposición −0.8 EV.
3. En el pie del módulo, elige **máscara dibujada y paramétrica**, y crea un **degradado**: arrástralo de arriba hacia el horizonte; ajusta la zona de transición.
4. En la parte paramétrica, en el canal **L** (luminosidad), deja pasar solo los tonos claros: así las montañas que sobresalen del horizonte no se oscurecen.
5. Activa *mostrar máscara* para verla en amarillo, y ajusta *difuminado* y *filtro guiado* hasta que no haya halo en las crestas.

### Cuándo usarlo y cuándo no
- **Úsalo** para equilibrar luz (cielo/suelo), dirigir la mirada al sujeto, corregir color en zonas, viñetas a medida y [[Dodge and burn]].
- **No lo uses** cuando un ajuste global resuelve el problema: más máscaras significan más riesgo de bordes visibles y más tiempo. Para comprimir luces y sombras en general, el ecualizador de tonos ya actúa "localmente" por brillo sin máscaras dibujadas.

### Errores comunes
1. **Bordes visibles o halos**: pasan cuando la máscara es dura o no sigue el contorno real (montañas, pelo, ramas). Se evitan con difuminado amplio, *filtro guiado*, rampas suaves en la máscara paramétrica y ajustes moderados: un −0.8 EV pasa desapercibido, un −3 EV no.
2. **Ajuste demasiado fuerte**: si notas el efecto, es excesivo. Baja la opacidad de la máscara.
3. **Olvidar zonas que cruzan la máscara**: una persona de pie frente al horizonte se oscurece con el cielo; resta su silueta de la máscara.
4. **Muchas instancias sin nombre**: renombra cada instancia ("cielo", "cara", "viñeta") para entender tu historial después.

### Para profundizar
- [darktable 5.6: módulos de procesamiento y fusión](https://docs.darktable.org/usermanual/5.6/en/module-reference/processing-modules/)
- [darktable 5.6: flujo (dodge and burn con exposición enmascarada)](https://docs.darktable.org/usermanual/5.6/en/overview/workflow/process/)
- [Novedades de darktable 5.6 (máscara de objeto por IA)](https://ubuntuhandbook.org/index.php/2026/06/darktable-released-5-6-0-with-optional-ai-support-heif-export/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un ajuste local?
> Un ajuste que se aplica solo a una zona de la foto (cielo, cara, fondo) mediante una máscara.

> [!question]- ¿Qué tipos de máscara tiene darktable?
> **Dibujadas** (pincel, círculo, elipse, trazo, degradado), **paramétricas** (por luminosidad, color o tono) y la combinación de ambas. Casi cualquier módulo acepta máscara.

> [!question]- ¿Cómo oscureces solo el cielo con un degradado?
> Agrega una instancia de **exposición** con máscara dibujada de degradado arriba, baja la exposición y, si el degradado toca edificios, combínala con una máscara paramétrica por luminosidad o tono azul.

> [!question]- Error común en máscaras: bordes visibles o halos. ¿Cómo se evitan?
> Usa difuminado (feather) amplio, refina la máscara (en darktable: suavizado o *guided filter* y el umbral de detalle) y haz ajustes moderados.

## Práctica

- [[Práctica Edición 05 - Máscaras locales y dodge and burn]]

