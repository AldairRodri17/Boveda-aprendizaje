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
# Qué es la impresión 3D FDM (y diferencias con resina y SLS)

Parte de: [[36-Impresion-3D/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La impresión 3D es **fabricación aditiva**: en lugar de quitar material (como un torno) se agrega capa por capa a partir de un modelo digital. **FDM** (*Fused Deposition Modeling*, también llamado **FFF**, *Fused Filament Fabrication*) usa un hilo de plástico termoplástico que se funde en una boquilla caliente y se deposita en líneas sobre una cama. Al terminar una capa, la cama baja (o el cabezal sube) y se imprime la siguiente. Es la tecnología de tu Centauri 2 y la más común en casa.

### Cómo funciona
```mermaid
flowchart LR
  M[Modelo 3D: STL/3MF/STEP] --> S[Laminador: rebana en capas]
  S --> G[G-code: trayectorias y temperaturas]
  G --> I[Impresora: funde y deposita]
  I --> P[Pieza + posprocesado]
```
1. Diseñas o descargas un modelo.
2. El laminador lo corta en capas y calcula por dónde pasa la boquilla (ver [[ElegooSlicer y OrcaSlicer]]).
3. La impresora sigue esas instrucciones: perímetros, relleno, soportes.
4. Retiras la pieza, quitas soportes y, si quieres, lijas y pintas.

### Comparativa con otras tecnologías

| | FDM / FFF | Resina (SLA, MSLA, DLP) | SLS |
|---|---|---|---|
| Material | Filamento termoplástico (PLA, PETG, TPU, ABS…) | Resina líquida fotosensible | Polvo (normalmente nylon) |
| Cómo endurece | Se funde y enfría | Luz UV cura cada capa | Láser sinteriza el polvo |
| Detalle | Medio; capas visibles (0.08–0.3 mm típico) | Muy alto (miniaturas, joyería) | Alto y uniforme |
| Soportes | Sí, en voladizos | Sí, casi siempre | No: el polvo sostiene |
| Limpieza | Mínima | Guantes, lavado con alcohol isopropílico, curado UV, residuos tóxicos | Despolvado, equipo industrial |
| Resistencia | Buena, piezas funcionales | Más frágil (según resina) | Muy buena |
| Costo en casa | Bajo | Medio | Industrial, caro |

En **resina**, una pantalla o láser endurece una capa entera de líquido: por eso el detalle es altísimo, pero el manejo químico es delicado. **SLS** da piezas funcionales excelentes sin soportes, pero las máquinas son de taller o industria. En casa lo normal es FDM o resina; esta área cubre solo FDM, así que las precauciones de resina no aplican a tu equipo.

### Ejemplo
Quieres dos cosas: un soporte para tu iPhone 15 Pro y unas figuras de 3 cm con caras muy finas.
- El **soporte** es perfecto para FDM: tamaño medio, necesita resistencia y lo imprimes en PETG o PLA en un par de horas.
- Las **figuras diminutas** con detalle fino lucirían mejor en resina. En tu FDM se pueden hacer con capa de 0.08–0.12 mm y, si quieres, boquilla de 0.2 mm, aceptando que se notarán líneas.

### Cuándo usar FDM y cuándo no
- **Sí**: piezas funcionales, organizadores, prototipos, carcasas, soportes para tu equipo de foto y dron, piezas grandes y multicolor con el CANVAS.
- **No tanto**: miniaturas con detalle de joyería, superficies perfectamente lisas sin lijar, piezas que deban ser herméticas sin tratamiento.

### Errores comunes
1. **"La impresión 3D es rápida."** Una pieza mediana tarda horas. Los 500 mm/s de la ficha son movimiento máximo, no el tiempo total: este depende del volumen, la altura de capa, perímetros, relleno, soportes y cambios de color.
2. **Esperar calidad de inyección.** FDM deja líneas de capa y una costura; el acabado fino se logra con [[Lijado y relleno de piezas impresas]].
3. **Pensar que todo se imprime sin soportes.** Los voladizos pronunciados necesitan apoyo u otra orientación: ver [[Soportes (normales, árbol e interfaces)]].
4. **Elegir tecnología por moda.** Decide por la pieza: tamaño, detalle, resistencia y cuánto posprocesado aceptas.

### Para profundizar
- [OrcaSlicer wiki — inicio](https://www.orcaslicer.com/wiki/)
- [Centauri 2 Combo — página oficial](https://us.elegoo.com/products/centauri-2-combo)
- Siguiente nota: [[Partes de una impresora FDM (CoreXY, extrusor, hotend y cama)]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es FDM/FFF?
> *Fused Deposition Modeling* (o *Fused Filament Fabrication*): un filamento termoplástico se funde en una boquilla caliente y se deposita en líneas, capa por capa, sobre una cama. La pieza se construye de abajo hacia arriba.

> [!question]- ¿En qué se diferencia de la impresión en resina (SLA/MSLA/DLP)?
> La resina usa un líquido fotosensible que se endurece con luz UV capa por capa. Da muchísimo más detalle (miniaturas), pero requiere guantes, lavado con alcohol isopropílico, curado UV y manejo de residuos tóxicos. FDM es más limpio, barato por pieza y da piezas más resistentes y grandes, con capas visibles.

> [!question]- ¿Y SLS?
> *Selective Laser Sintering*: un láser sinteriza polvo (normalmente nylon). No necesita soportes porque el polvo sostiene la pieza; da piezas funcionales muy buenas, pero las máquinas son industriales y caras. Para casa, lo normal es FDM o resina.

> [!question]- Quieres imprimir un soporte para tu iPhone y unas figuras de 3 cm muy detalladas. ¿Qué tecnología conviene para cada una?
> Soporte de iPhone → FDM (resistente, barato, tamaño medio). Figuras de 3 cm con detalle fino → resina sería mejor; en FDM se puede con boquilla fina y capa baja, pero se notarán las capas.

> [!question]- Error común: «la impresión 3D es rápida».
> Una pieza mediana tarda horas. La velocidad de la impresora (mm/s) no es el tiempo total: el tiempo depende del volumen, altura de capa, perímetros, relleno, soportes y, en multicolor, de los cambios de filamento.

