---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Impresión 3D
tags: [impresion-3d]
contenido: completo
---
# Adherencia a la cama (brim, raft y superficies)

Parte de: [[36-Impresion-3D/02 Laminador/_Índice Laminador|Laminador]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **adherencia a la cama** es que la primera capa se quede pegada durante toda la impresión y se suelte al final. Depende de cuatro cosas: la **superficie** de la placa, su **limpieza**, la **temperatura** de la cama y la **primera capa** (altura y aplastado). El laminador agrega ayudas opcionales: **skirt**, **brim** y **raft**.

### Cómo funciona
**Superficies de tu placa.** La Centauri 2 trae una placa flexible de **doble cara**; lo habitual es una cara **lisa** y otra de **PEI texturizado** (confirma en el manual cuáles trae la tuya: Pendiente de confirmar). La lisa deja la cara inferior brillante; la texturizada deja un acabado granulado y suelta muy bien el PETG.

**Ayudas del laminador:**

| Ayuda | Qué es | Cuándo |
|---|---|---|
| *Skirt* | Una línea alrededor, sin tocar la pieza | Purgar la boquilla y ver la primera capa |
| *Brim* | Varias líneas pegadas al borde de la pieza (como ala de sombrero) | Piezas altas y delgadas, esquinas agudas, PETG/ABS/ASA |
| *Brim ears* | Discos solo en las esquinas | Esquinas que se levantan, sin brim completo |
| *Raft* | Plataforma de varias capas debajo de la pieza | Casi nunca hoy; camas muy irregulares o materiales difíciles |

El **raft** se usaba cuando las camas estaban desniveladas: la pieza se apoyaba sobre una base gruesa que absorbía el error. Con nivelación automática y Z offset automático, como tu impresora, rara vez hace falta; además gasta material y deja la cara inferior áspera.

**Temperaturas de cama orientativas** (consulta siempre la ficha del fabricante del filamento): PLA 55–65 °C, PETG 70–85 °C, TPU 30–60 °C, ABS/ASA 90–110 °C.

### Ejemplo
Imprimes una torre delgada de 15 cm (un soporte para la DJI Mic Mini 2). Con base pequeña y mucha altura, el movimiento rápido puede despegarla. Agregas un **brim** de 5 mm, usas la cara texturizada si es PETG, lavas la placa antes y dejas que la impresora haga la nivelación. Al terminar, el brim se desprende con los dedos o se recorta.

**Limpieza:** lava la placa con agua tibia y jabón para trastes, enjuaga y seca con papel sin pelusa; entre impresiones puedes pasar alcohol isopropílico. Tómala siempre por los bordes: la grasa de los dedos es la causa número uno de piezas que se despegan justo en una zona.

### Cuándo usar cada cosa
- Pieza grande y plana en PLA → cara lisa, sin brim.
- Esquinas agudas o material que contrae → brim o brim ears.
- PETG → cara texturizada; en la lisa, una capa de barra de pegamento como **separador**.
- Pieza con poca base → reoriéntala antes de pensar en raft.

### Errores comunes
1. **PETG sobre placa lisa muy limpia y caliente.** Se pega **demasiado** y puede arrancar pedazos de la superficie. Usa la texturizada o pegamento en barra como separador.
2. **Tocar la placa recién lavada.** Grasa = despegue local.
3. **Subir la cama "para que pegue más" sin límite.** Con PLA, la cama muy caliente provoca [[Pata de elefante en FDM]] y puede ablandar la base.
4. **Quitar la pieza en caliente doblando fuerte.** Espera a que enfríe; la placa flexible se deforma si la doblas de más.
5. **Usar raft por costumbre.** Pierdes tiempo y acabado.

### Para profundizar
- [OrcaSlicer wiki: pintar brim ears](https://www.orcaslicer.com/wiki/print_prepare/prepare_brim_ears_painting)
- [OrcaSlicer wiki: skirt](https://www.orcaslicer.com/wiki/print_settings/others/others_settings_skirt)
- Relacionado: [[Primera capa y calibración automática]] y [[Warping (esquinas levantadas) en FDM]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un brim y cuándo usarlo?
> Un borde de líneas alrededor de la primera capa que aumenta el área de contacto. Úsalo en piezas altas y delgadas, con esquinas agudas, o con materiales que se levantan (PETG, ABS).

> [!question]- ¿Qué es un raft y por qué casi no se usa ya?
> Una plataforma de varias capas bajo la pieza. Ayudaba con camas desniveladas; con nivelación automática y placas texturizadas casi nunca hace falta, y deja la cara inferior áspera.

> [!question]- ¿Por qué limpiar la placa?
> La grasa de los dedos arruina la adherencia. Lava la placa con agua y jabón para trastes (o alcohol isopropílico) y tómala de los bordes.

> [!question]- Error común: PETG sobre placa lisa de PEI muy limpia y caliente.
> El PETG puede pegarse **demasiado** y arrancar pedazos de la superficie. Usa la cara texturizada o un agente separador (barra de pegamento) en lisa.

## Práctica

- [[Lab 1 - Primera impresión y revisión de calibración automática en Centauri 2]]

