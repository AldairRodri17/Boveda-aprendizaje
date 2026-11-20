---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Impresión 3D
tags: [impresion-3d]
---
# Filamentos especiales (seda, mate, madera y brillo)

Parte de: [[36-Impresion-3D/03 Materiales/_Índice Materiales|Materiales]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son filamentos, casi siempre con base de **PLA**, a los que se agregan aditivos para cambiar el **aspecto**: brillo metálico, acabado mate, textura de madera, destellos o brillo en la oscuridad. Se usan sobre todo en figuras y piezas decorativas. Cambian un poco la forma de imprimir y algunos desgastan la boquilla.

### Cómo funciona

| Tipo | Qué tiene | Efecto | Cómo imprimirlo |
|---|---|---|---|
| **Seda (Silk)** | Aditivos que alinean el plástico y reflejan la luz | Brillo metálico satinado, oculta capas a la vista | Algo más de temperatura y **menos velocidad** en la pared exterior; es menos resistente entre capas |
| **Mate** | Cargas minerales o aditivos que dispersan la luz | Superficie sin brillo que **esconde mucho las líneas de capa** | Similar a PLA; un poco más quebradizo |
| **Madera** | Fibras o polvo de madera | Aspecto y olor a madera; se puede lijar y teñir | Boquilla ≥0.4 mm, evitar temperaturas excesivas (se quema y oscurece) |
| **Glitter / Sparkle / Galaxy** | Partículas brillantes | Destellos | Boquilla ≥0.4 mm |
| **Brillo en la oscuridad (Glow)** | Pigmentos fosforescentes (minerales duros) | Brilla tras cargar con luz | **Muy abrasivo**: boquilla endurecida |
| **Mármol** | Partículas de color | Moteado tipo piedra | Boquilla ≥0.4 mm |

**El riesgo de las partículas.** Los filamentos con cargas (madera, glitter, glow, también los de fibra de carbono) pueden:
1. **Desgastar** la boquilla: las de latón se agrandan y pierden precisión; según el perfil de OrcaSlicer la tuya es de **acero endurecido** (Pendiente de confirmar en la ficha oficial), lo que ayudaría mucho.
2. **Tapar** boquillas finas: con 0.2 mm las partículas se atoran. Usa 0.4 mm o mayor.

Los perfiles de Elegoo para la Centauri 2 en OrcaSlicer incluyen PLA Silk, Matte, Wood, Sparkle, Galaxy, Marble y Glow: buen punto de partida, ajustando con la ficha del fabricante del filamento.

### Ejemplo
Para una figura de exhibición que no vas a pintar, eliges **PLA mate**: las capas casi no se notan en fotos con tu Sony a6700. Para un trofeo con aspecto de metal, **PLA seda** dorado: bajas la velocidad de pared exterior y haces una torre de temperatura, porque la seda cambia mucho de brillo según la temperatura (más caliente suele dar más brillo, pero más hilos). Para un letrero que brille de noche, **Glow** con boquilla de 0.4 mm y confirmando antes que tu boquilla sea endurecida; aun así, revisa el desgaste de vez en cuando.

### Cuándo usarlos y cuándo no
- **Sí**: piezas visuales, regalos, figuras, letreros, prototipos de presentación.
- **No**: piezas mecánicas (seda y mate son más débiles entre capas), boquillas finas con partículas.
- **Multicolor**: funcionan con el CANVAS; ten en cuenta que los de alto contraste necesitan más purga.

### Errores comunes
1. **Usar boquilla de 0.2 mm con madera o glitter.** Atascos casi seguros.
2. **Imprimir seda rápido.** El brillo se vuelve irregular y la pared exterior muestra bandas.
3. **Esperar resistencia de PLA normal.** Los aditivos debilitan; usa PLA o PETG para piezas funcionales.
4. **Madera muy caliente o muy lenta.** Se carboniza y deja manchas oscuras o atasca.
5. **No recalibrar**: cada filamento especial necesita su torre de temperatura y su flujo.

### Para profundizar
- [OrcaSlicer wiki: calibración de temperatura](https://www.orcaslicer.com/wiki/calibration/temp_calib)
- [OrcaSlicer wiki: calibración de flujo](https://www.orcaslicer.com/wiki/calibration/flow_ratio_calib)
- Relacionado: [[PLA]] y [[Atascos de boquilla en FDM]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué tiene de especial el PLA seda (silk)?
> Aspecto brillante metálico; se ve muy bien en figuras, pero es menos resistente entre capas y le gusta una temperatura algo más alta y velocidad menor.

> [!question]- ¿Qué riesgo tienen los filamentos con partículas (madera, glitter, brillo en la oscuridad)?
> Son **abrasivos** o tienen partículas que pueden tapar boquillas finas. Usa boquilla de 0.4 mm o mayor (mejor de acero endurecido) y evita 0.2 mm.

> [!question]- ¿Para qué sirve el PLA mate?
> Esconde mucho las líneas de capa, ideal para piezas que se verán y no se pintarán.

