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
# Calibrador digital (vernier)

Parte de: [[36-Impresion-3D/06 Herramientas y compras/_Índice Herramientas y compras|Herramientas y compras]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **calibrador digital** (también llamado vernier o pie de rey) mide distancias con resolución de 0.01 mm: exteriores, interiores, profundidades y escalones. En impresión 3D es la herramienta que conecta el mundo real con tus diseños: sin medir, no puedes diseñar piezas que encajen ni verificar si tu impresora es precisa.

### Cómo funciona
Tiene cuatro formas de medir:

| Parte | Mide | Ejemplo |
|---|---|---|
| **Mandíbulas exteriores** (grandes, abajo) | Exteriores, espesores | Grosor de una pared, diámetro de un perno |
| **Mandíbulas interiores** (pequeñas, arriba) | Interiores | Diámetro de un agujero, ancho de una ranura |
| **Varilla de profundidad** (atrás) | Profundidades | Profundidad de un agujero para inserto |
| **Escalón** (parte trasera de las mandíbulas) | Diferencias de altura | Altura de un relieve |

**Resolución vs. exactitud.** Muestra 0.01 mm, pero los calibradores económicos suelen tener una exactitud real menor (del orden de ±0.02–0.03 mm según el fabricante). Para FDM es suficiente: las tolerancias de impresión son de décimas.

**Cómo medir bien:**
1. **Pon en cero** con las mandíbulas cerradas y limpias.
2. Mide **sin apretar fuerte**: el plástico cede y la lectura baja.
3. Mide en **varios puntos** y a **media altura** de la pieza.
4. Mantén el calibrador **perpendicular** a lo que mides.
5. Para agujeros, usa las mandíbulas interiores y busca la lectura **máxima** girando un poco.
6. Cambia la pila cuando la lectura "salte"; ciérralo al guardarlo para proteger las puntas.

```mermaid
flowchart LR
  A[Objeto real: iPhone, Osmo Pocket] -->|Medir| B[Cotas en el diseño]
  B --> C[Agregar holguras]
  C --> D[Imprimir]
  D -->|Medir de nuevo| E[Ajustar diseño o compensación]
```

### Ejemplo
Diseñas un soporte para tu iPhone 15 Pro con funda. Mides el grosor del teléfono con funda en tres puntos (por ejemplo 11.2, 11.3, 11.2 mm) y usas el mayor. Agregas tu holgura deslizante de la [[Prueba de tolerancias]] (0.2 mm por lado) → la ranura es de 11.3 + 0.4 = 11.7 mm. Imprimes y mides la ranura con las mandíbulas interiores: 11.6 mm. Sabes que tu impresora cierra un poco los huecos y lo anotas para el siguiente diseño.

También lo usas para calibrar flujo: mides el espesor de una pared simple (ver [[Calibración de flujo y pressure advance]]).

### Cuándo usarlo
- Antes de diseñar: medir el objeto real.
- Después de imprimir: verificar medidas y tolerancias.
- Para filamentos: medir el diámetro (debería estar cerca de 1.75 mm en varios puntos).

### Errores comunes
1. **Medir sobre la costura o la pata de elefante.** Te da medidas mayores; mide a media altura y lejos de la costura. Ver [[Pata de elefante en FDM]].
2. **Apretar demasiado**: deforma el plástico (sobre todo TPU).
3. **No poner en cero**: todas las lecturas quedan desplazadas.
4. **Medir agujeros con las mandíbulas exteriores**.
5. **Usarlo como herramienta** (rayar, palanca): pierde exactitud.

### Para profundizar
- [OrcaSlicer wiki: calibración de tolerancias](https://www.orcaslicer.com/wiki/calibration/tolerance_calib)
- Laboratorio: [[Lab 4 - Prueba de tolerancias con pieza que encaja]]
- Relacionado: [[Herramientas básicas para impresión 3D]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve en impresión 3D?
> Medir agujeros, ejes y espesores para diseñar piezas que encajen, verificar tolerancias y calibrar flujo con una pared.

> [!question]- ¿Cómo medir bien?
> Pon en cero con las mandíbulas cerradas, mide sin apretar fuerte (el plástico cede), en varios puntos, y usa las mandíbulas internas para agujeros y la varilla para profundidades.

> [!question]- Error común: medir sobre la costura o la pata de elefante.
> Te da medidas mayores; mide a media altura y lejos de la costura.

## Práctica

- [[Lab 4 - Prueba de tolerancias con pieza que encaja]]

