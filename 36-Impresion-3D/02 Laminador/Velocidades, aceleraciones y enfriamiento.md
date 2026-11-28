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
# Velocidades, aceleraciones y enfriamiento

Parte de: [[36-Impresion-3D/02 Laminador/_Índice Laminador|Laminador]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **velocidad** (mm/s) es qué tan rápido se mueve la boquilla mientras extruye o viaja. La **aceleración** (mm/s²) es qué tan rápido llega a esa velocidad. El **enfriamiento** es cuánto sopla el ventilador de capa sobre el plástico recién puesto. Los tres juntos deciden el tiempo de impresión, la calidad de las paredes y la resistencia entre capas.

### Cómo funciona
**Por qué casi nunca llegas a 500 mm/s.** Tu Centauri 2 anuncia 500 mm/s y 20,000 mm/s², pero:
1. **Distancia para acelerar.** Para llegar de 0 a 500 mm/s a 20,000 mm/s² se necesitan v²/(2a) = 500²/(2 × 20,000) = 6.25 mm, y otro tanto para frenar. En una pieza con tramos cortos (detalles, relleno pequeño) el cabezal nunca alcanza la velocidad máxima.
2. **Flujo volumétrico.** El hotend solo funde cierto volumen por segundo (Elegoo no lo publica para la Centauri 2: Pendiente de confirmar; en la práctica depende del filamento). Por ejemplo, con capa 0.2 y línea 0.45, un filamento que funde 20 mm³/s permite ≈222 mm/s; un PLA común puede limitar antes y uno de alta velocidad después. El laminador baja la velocidad automáticamente si superas la **velocidad volumétrica máxima** del perfil de filamento. Ver [[Altura de capa y ancho de línea]].

**Velocidades por tipo de línea.** No todo va igual de rápido:
- **Pared exterior**: la más lenta; define la apariencia.
- **Paredes internas y relleno**: rápidas; no se ven.
- **Primera capa**: lenta, para que pegue.
- **Puentes y voladizos**: con su propia velocidad y ventilador.

**Ringing / ghosting.** Ondulaciones que aparecen después de esquinas: el marco y el cabezal vibran al frenar. Si las ves, baja la **velocidad y la aceleración de la pared exterior**. Los firmware modernos aplican *input shaping* para compensar resonancias; si tu impresora lo calibra sola está (Pendiente de confirmar en el manual).

**Ventilador de capa según material** (rangos generales; consulta la ficha del fabricante del filamento):

| Material | Ventilador de capa |
|---|---|
| PLA | Alto: 80–100 % tras las primeras capas |
| PETG | Medio: 30–60 %; demasiado = capas débiles |
| TPU | Bajo a medio |
| ABS / ASA | Muy bajo o apagado; idealmente en impresora cerrada |

Las primeras 1–3 capas suelen ir con el ventilador apagado o bajo para no enfriar la base.

**Tiempo mínimo de capa.** En capas pequeñas (punta de una torre), el laminador baja la velocidad para que cada capa tenga tiempo de enfriarse antes de la siguiente.

### Ejemplo
Imprimes una carcasa en PLA y la pared exterior muestra ondas junto a las letras grabadas. Bajas la velocidad de pared exterior de 200 a 120 mm/s y su aceleración a la mitad; el resto queda igual. El tiempo total sube poco porque el relleno y las paredes internas siguen rápidos. Si además el techo tiene huecos, revisas la velocidad volumétrica máxima del perfil.

### Cuándo bajar y cuándo subir
- **Bajar**: pared exterior visible, TPU, filamento silk, piezas altas y delgadas, voladizos difíciles.
- **Subir**: prototipos, relleno, filamentos "high speed" con su perfil del fabricante.
- **Ventilador**: sube si los voladizos se caen; baja si las capas se separan (PETG, ABS).

### Errores comunes
1. **Subir solo la velocidad** sin mirar el flujo: subextrusión.
2. **PETG con ventilador al 100 %**: capas que se separan con poca fuerza.
3. **ABS con ventilador alto**: warping y grietas (y en tu impresora abierta, aún peor).
4. **Cambiar aceleraciones del perfil de impresora** sin necesidad: los valores de fábrica ya consideran la mecánica.

### Para profundizar
- [OrcaSlicer wiki: calibración de velocidad volumétrica máxima](https://www.orcaslicer.com/wiki/calibration/volumetric_speed_calib)
- [OrcaSlicer wiki: calibración de input shaping](https://www.orcaslicer.com/wiki/calibration/input_shaping_calib)
- [OrcaSlicer wiki: guía de calibración](https://www.orcaslicer.com/wiki/guides/calibration_guide.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué la velocidad máxima (500 mm/s) casi nunca se alcanza?
> En trayectos cortos el cabezal no tiene distancia para acelerar, y el límite real suele ser el **flujo volumétrico** que el hotend puede fundir, no el movimiento.

> [!question]- ¿Qué velocidad bajar si las paredes exteriores se ven onduladas (ringing)?
> La de **pared exterior** y su aceleración. La pared exterior define la apariencia; las internas y el relleno pueden ir rápido.

> [!question]- ¿Cómo se ajusta el ventilador de capa según material?
> PLA: ventilador alto (casi 100 % tras las primeras capas). PETG: medio (30–60 %), si no pierde adhesión entre capas. TPU: bajo-medio. ABS/ASA: muy bajo o apagado.

