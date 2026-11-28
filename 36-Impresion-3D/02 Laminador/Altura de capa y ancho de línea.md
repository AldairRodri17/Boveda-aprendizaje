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
# Altura de capa y ancho de línea

Parte de: [[36-Impresion-3D/02 Laminador/_Índice Laminador|Laminador]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **altura de capa** es el grosor vertical de cada capa (por ejemplo 0.2 mm). El **ancho de línea** es lo ancho que queda cada cordón de plástico depositado (por ejemplo 0.42 mm). Juntos definen la resolución, la resistencia y, sobre todo, el **tiempo** de impresión y el **flujo** que debe fundir el hotend.

### Cómo funciona
**Número de capas.** Se divide la altura de la pieza entre la altura de capa:
- 30 mm ÷ 0.2 mm = **150 capas**.
- 30 mm ÷ 0.12 mm = **250 capas**: 100 capas más, y por tanto mucho más tiempo.

**Escalones.** En curvas y pendientes suaves, cada capa es un escalón; capas finas = escalones menos visibles. En paredes verticales casi no hay diferencia.

**Regla de la altura máxima.** No pases de ~75–80 % del diámetro de boquilla: con 0.4 mm, unos 0.3 mm. Más gruesa y la línea ya no se aplasta contra la capa anterior: mala unión entre capas.

**Ancho de línea.** Suele ser un poco **mayor que la boquilla**: con 0.4 mm se usan 0.42–0.45 mm. La boquilla aplasta el plástico y lo extiende; esa línea más ancha se funde mejor con la vecina y la de abajo, dando paredes más fuertes.

**Flujo volumétrico.** Lo que el hotend debe fundir por segundo:

```text
flujo (mm³/s) = altura de capa × ancho de línea × velocidad
             = 0.2 mm × 0.42 mm × 200 mm/s = 16.8 mm³/s
```
Elegoo no publica en la ficha de la Centauri 2 un flujo máximo (Pendiente de confirmar); en la práctica el límite depende sobre todo del filamento (un PLA de alta velocidad aguanta más que uno común; el TPU, mucho menos). Si pides más flujo del que el filamento puede fundir, hay subextrusión. Ver [[Velocidades, aceleraciones y enfriamiento]].

**Cuánto filamento hay en un carrete.** Con filamento de 1.75 mm y PLA de densidad 1.24 g/cm³:

```text
volumen de 1 kg   = 1000 g ÷ 1.24 g/cm³ ≈ 806 cm³
área de sección   = π × (0.0875 cm)² ≈ 0.02405 cm²
longitud          = 806 ÷ 0.02405 ≈ 33,500 cm ≈ 335 m
```

### Ejemplo
Para una caja organizadora (funcional, paredes rectas): 0.2 mm de capa, 0.42–0.45 mm de ancho. Para una figura con caras redondeadas: 0.12 mm o, mejor, [[Costura, ironing y altura de capa variable|altura de capa variable]], que pone capas finas solo en las curvas. Para un prototipo rápido: 0.28 mm, aceptando que se noten las líneas.

| Altura | Uso típico (boquilla 0.4 mm) |
|---|---|
| 0.08–0.12 mm | Figuras, detalles finos, texto pequeño |
| 0.16–0.20 mm | Equilibrio general; el perfil estándar |
| 0.24–0.28 mm | Prototipos y piezas grandes, rápido |

### Cuándo cambiarla
- Baja la capa cuando la pieza es **visual** y curva.
- Súbela cuando la pieza es **funcional** y grande.
- Si necesitas más resistencia, mejor agrega paredes que bajar capa: ver [[Perímetros y relleno]].

### Errores comunes
1. **Bajar a 0.08 mm "para más calidad" en una pieza funcional.** Casi triplica el tiempo y no la hace más fuerte; muchas veces la debilita por acumulación de calor en piezas pequeñas.
2. **Capa de 0.32 mm con boquilla de 0.4.** Pasa el límite; capas que se separan.
3. **Subir velocidad sin mirar el flujo.** El resultado es subextrusión aunque la máquina "pueda" ir más rápido.
4. **Medir piezas con primera capa distinta.** La primera capa suele ser más gruesa a propósito para pegar mejor; considéralo en piezas que encajan.

### Para profundizar
- [OrcaSlicer wiki: altura de capa](https://www.orcaslicer.com/wiki/print_settings/quality/quality_settings_layer_height)
- [OrcaSlicer wiki: ancho de línea](https://www.orcaslicer.com/wiki/print_settings/quality/quality_settings_line_width)
- [OrcaSlicer wiki: calibración de velocidad volumétrica máxima](https://www.orcaslicer.com/wiki/calibration/volumetric_speed_calib)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Cuántas capas tiene una pieza de 30 mm de alto con capa de 0.2 mm? ¿Y con 0.12 mm?
> 30 / 0.2 = **150 capas**; 30 / 0.12 = **250 capas** (≈67 % más capas, y aproximadamente más tiempo).

> [!question]- Calcula el flujo volumétrico con capa 0.2 mm, ancho 0.42 mm y 200 mm/s.
> 0.2 × 0.42 × 200 = **16.8 mm³/s**. Si el hotend no puede fundir tanto (límite del perfil de filamento), el laminador baja la velocidad o habrá subextrusión.

> [!question]- ¿Cuánto filamento de 1.75 mm hay en 1 kg de PLA (densidad 1.24 g/cm³)?
> Área = π × 0.875² ≈ 2.405 mm². Volumen = 1000 g / 0.00124 g/mm³ ≈ 806,452 mm³. Longitud ≈ 806,452 / 2.405 ≈ 335,300 mm ≈ **335 m** (≈2.98 g por metro).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué efecto tiene la altura de capa?
> Capa baja (0.12 mm) = más detalle y menos escalones en curvas, pero más tiempo. Capa alta (0.28 mm) = más rápido, más visible. Con boquilla de 0.4 mm lo típico es 0.2 mm.

> [!question]- Regla práctica para la altura máxima de capa
> No pasar de ~75–80 % del diámetro de la boquilla (≈0.3 mm con 0.4 mm), o las capas se pegan mal.

> [!question]- ¿Qué es el ancho de línea y por qué suele ser mayor que la boquilla?
> Es el ancho real de cada línea depositada. Con 0.4 mm se usan 0.42–0.45 mm: la línea aplastada pega mejor y da paredes más fuertes.

> [!question]- Error común: bajar la capa a 0.08 mm para «más calidad» en una pieza funcional.
> Triplica el tiempo y casi no mejora la resistencia. Usa capa fina solo donde se ve la diferencia (figuras, curvas suaves) o usa altura de capa variable.

