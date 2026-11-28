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
# PLA

Parte de: [[36-Impresion-3D/03 Materiales/_Índice Materiales|Materiales]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **PLA** (ácido poliláctico) es el filamento más usado en FDM. Es un termoplástico de origen vegetal (almidón de maíz o caña), fácil de imprimir, barato, con poco olor y disponible en cientos de colores y acabados. Es el material con el que debes aprender a usar tu Centauri 2.

### Cómo funciona
**Por qué es fácil:**
- Funde a temperatura baja: boquilla ≈190–230 °C, cama ≈55–65 °C (rango general; consulta la ficha del fabricante del filamento).
- **Casi no se contrae** al enfriar: poco *warping*, no necesita gabinete caliente.
- Solidifica rápido con el ventilador: voladizos y puentes salen bien.
- Emite menos partículas y VOCs que ABS o nylon.

**Limitaciones:**
- **Temperatura**: se ablanda alrededor de 55–60 °C (temperatura de transición vítrea). Dentro de un coche al sol o junto a algo caliente se deforma.
- **Rígido y quebradizo**: resiste bien la carga estática, pero se rompe en vez de doblarse ante golpes.
- Con el tiempo y humedad puede volverse más frágil.

**Variantes:** no todos los "PLA" son iguales; los aditivos cambian la temperatura y el comportamiento.

| Variante | Diferencia |
|---|---|
| PLA básico | Referencia |
| PLA+ / PLA Pro | Aditivos para más tenacidad; suele pedir 5–15 °C más |
| PLA de alta velocidad (Rapid, HS) | Funde más rápido; permite más flujo volumétrico |
| PLA Silk, Mate, Madera, Glow | Ver [[Filamentos especiales (seda, mate, madera y brillo)]] |
| PLA-CF | Con fibra de carbono; abrasivo, con boquilla endurecida (la tuya lo es según el perfil de OrcaSlicer: Pendiente de confirmar) |

**En tu impresora abierta.** La Centauri 2 no tiene gabinete, y eso le conviene al PLA: prefiere aire fresco y enfriamiento rápido. Lo que sí debes evitar son corrientes directas (ventilador, aire acondicionado) que enfríen la pieza de un solo lado y provoquen warping en piezas grandes.

### Ejemplo
Tu primer proyecto: un organizador de escritorio para cables y la DJI Mic Mini 2. PLA Elegoo con RFID en el CANVAS → la impresora reconoce el material y aplica ajustes. Cara lisa de la placa, 0.2 mm de capa, 3 paredes, 15 % de relleno. Si una pieza sale con **hilos y brillo raro**, revisas primero: temperatura (quizá muy alta → [[Torre de temperatura]]) y humedad del filamento (→ [[Secado y almacenamiento de filamento]]).

### Cuándo usarlo y cuándo no
- **Sí**: figuras, prototipos, organizadores, maquetas, piezas multicolor, soportes de escritorio, piezas que se pintarán.
- **No**: piezas en el coche, exteriores al sol, cerca de motores o lámparas calientes, piezas que se doblan o reciben impactos. Para eso, [[PETG]] o, si hace falta más, ASA (ver [[Materiales que necesitan impresora cerrada (ABS, ASA, PC y nylon)]]).

### Errores comunes
1. **Asumir que PLA+ o Silk se imprimen igual que PLA.** Tienen aditivos: haz su propia torre de temperatura y perfil.
2. **Dejar piezas de PLA en el coche.** Se deforman en una tarde de calor.
3. **Ventilador bajo con PLA.** Voladizos caídos y detalles derretidos; PLA quiere ventilador alto tras las primeras capas.
4. **Guardar el carrete al aire libre meses.** Aunque es menos sensible que PETG o TPU, también absorbe humedad.
5. **Cama demasiado caliente.** Provoca [[Pata de elefante en FDM]].

### Para profundizar
- [OrcaSlicer wiki: calibración de temperatura](https://www.orcaslicer.com/wiki/calibration/temp_calib)
- [OrcaSlicer wiki: guía de calibración](https://www.orcaslicer.com/wiki/guides/calibration_guide.html)
- [Centauri 2 — página oficial](https://us.elegoo.com/products/centauri-2)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué PLA es el material para empezar?
> Imprime a temperatura baja (≈190–220 °C boquilla, 55–65 °C cama), casi no se deforma, tiene poco olor y es barato. Ideal para figuras, prototipos y organizadores.

> [!question]- ¿Cuál es su limitación principal?
> Se ablanda alrededor de 55–60 °C: dentro de un coche al sol o cerca de algo caliente se deforma. También es rígido y frágil al impacto.

> [!question]- Una pieza de PLA salió con hilos y brillo raro, ¿qué revisas primero?
> Temperatura (quizá muy alta: haz [[Torre de temperatura]]) y humedad del filamento (ver [[Secado y almacenamiento de filamento]]).

> [!question]- ¿PLA+ o PLA Silk son lo mismo que PLA?
> No exactamente: tienen aditivos. Suelen requerir un poco más de temperatura y su propia calibración.

## Práctica

- [[Lab 1 - Primera impresión y revisión de calibración automática en Centauri 2]]
- [[Lab 2 - Torre de temperatura de PLA]]

