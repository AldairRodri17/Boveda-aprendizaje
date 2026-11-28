---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Modelado 3D técnico
tags: [modelado-3d-tecnico]
contenido: completo
---
# Chaflanes, redondeos, vaciado y patrones

Parte de: [[37-Modelado-3D-Tecnico/02 Modelado paramétrico/_Índice Modelado paramétrico|Modelado paramétrico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son las **operaciones de acabado** del modelado paramétrico: se aplican sobre un sólido que ya existe, en lugar de dibujarse en el boceto.

- **Chaflán** (*chamfer*): corta una arista con un plano inclinado (normalmente a 45°).
- **Redondeo** (*fillet*): reemplaza una arista por una superficie curva de radio dado.
- **Vaciado** (*shell*): ahueca un sólido dejando paredes de grosor fijo y quitando las caras que elijas.
- **Patrón** (*pattern*): repite una operación o un cuerpo en línea, en círculo o a lo largo de una curva.

Bien usadas hacen que una pieza se vea profesional, sea más resistente y se imprima mejor.

### Cómo funciona
Las cuatro dependen de la geometría que ya está en el historial, por eso conviene ponerlas **al final** de la lista de operaciones: si cambias el boceto base, se recalculan sobre la forma nueva.

```mermaid
flowchart LR
  A[Boceto simple] --> B[Extruir]
  B --> C[Vaciado]
  C --> D[Agujeros / ranuras]
  D --> E[Patrón de ranuras]
  E --> F[Redondeos verticales y superiores]
  F --> G[Chaflán en la base]
```

**Pensando en la impresión FDM**:

- **Borde que toca la cama → chaflán**. Un redondeo inferior empieza casi horizontal: es un voladizo que se imprime feo y con capas colgando. Un chaflán de 45° cumple la [[Voladizos y puentes|regla de 45°]] y, de paso, absorbe la **pata de elefante** (la primera capa que se aplasta y sale más ancha). Un chaflán de 0.4–0.6 mm basta.
- **Redondeos** en aristas verticales (se imprimen como curvas en XY, perfectas) y en aristas superiores. En esquinas interiores, un redondeo reparte esfuerzos y evita grietas.
- **Vaciado**: el grosor debe ser **múltiplo del ancho de línea**. Con boquilla de 0.4 mm (la que se asume de fábrica en tu [[Elegoo Centauri 2 Combo]]; confírmalo) y línea de 0.42 mm: 1.26 mm (3 líneas), 1.68 mm (4 líneas), 2.1 mm (5 líneas). Ver [[Grosor de pared y relleno según la carga]].
- **Patrones**: lineal para ranuras de ventilación o divisiones; circular para agujeros de una brida o dientes de una perilla; por curva para texturas que siguen un borde. La cantidad queda en un solo parámetro.

### Ejemplo
Perilla de 30 mm para un potenciómetro, en Onshape o Fusion:

1. Boceto de un círculo Ø30, extruir 15 mm.
2. **Vaciado** desde la cara inferior con 1.68 mm (la perilla queda hueca y ligera).
3. Boceto de una ranura semicircular Ø4 en el borde, extruir-cortar todo el alto.
4. **Patrón circular** de la ranura: 18 copias a 360°. Esa variable (`#n_ranuras`) cambia el agarre en segundos.
5. **Redondeo** de 1.5 mm en la arista superior exterior.
6. **Chaflán** de 0.5 mm en la arista inferior exterior.

En [[OpenSCAD]] el patrón es un `for`:

```openscad
n = 18;
difference() {
  cylinder(d = 30, h = 15, $fn = 96);
  for (i = [0 : n - 1])
    rotate([0, 0, i * 360 / n]) translate([15, 0, -1]) cylinder(d = 4, h = 17, $fn = 24);
  translate([0, 0, -0.01]) cylinder(d = 30 - 2*1.68, h = 15 - 1.68, $fn = 96);  // vaciado
}
```

### Práctica en tu Centauri 2
Imprime dos cubos de prueba de 20 mm: uno con **redondeo** de 3 mm en todas las aristas inferiores y otro con **chaflán** de 3 mm. Compáralos: el redondeado mostrará capas colgadas en el primer milímetro; el chaflán saldrá limpio. Guarda las piezas como referencia visual.

### Cuándo usarlo y cuándo no
Úsalos siempre que mejoren la impresión o la resistencia. Evita redondeos grandes en la base (usa chaflán), redondeos diminutos de 0.2 mm (no se notan con capa de 0.2 y complican el modelo) y vaciados en piezas que deben soportar carga: ahí es mejor una pieza sólida y dejar que el laminador decida perímetros y relleno.

### Errores comunes
- **Redondeo antes que el vaciado**: el vaciado falla o deja paredes irregulares. Primero vaciado, luego redondeos.
- **Redondeo mayor que la pared**: el CAD no puede calcularlo y la operación da error.
- **Patrones de geometría ya redondeada**: es más estable patronar la operación simple y redondear después.
- **Grosor de vaciado arbitrario** (1.0, 1.5): el laminador mete líneas delgadas o huecos en la pared.

### Para profundizar
- [PartDesign Fillet (wiki de FreeCAD)](https://wiki.freecad.org/PartDesign_Fillet)
- [PartDesign Thickness, el vaciado de FreeCAD](https://wiki.freecad.org/PartDesign_Thickness)
- [Onshape Learning Center](https://learn.onshape.com/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué en el borde que toca la cama conviene un chaflán y no un redondeo?
> El redondeo inferior empieza casi horizontal: es un **voladizo** que se imprime feo. Un chaflán de 45° se imprime limpio y además compensa la "pata de elefante" de la primera capa. Redondeos sí en bordes verticales y superiores.

> [!question]- ¿Qué hace el vaciado (*shell*) y qué grosor le pones?
> Ahueca un sólido dejando paredes de un grosor dado y quitando las caras que elijas (la tapa de una caja). Usa un **múltiplo del ancho de línea**: con boquilla de 0.4 mm, 1.6 mm ≈ 4 líneas, 2.0 mm ≈ 5 líneas (ver [[Grosor de pared y relleno según la carga]]).

> [!question]- ¿Patrón lineal, circular o por curva? Ejemplos.
> **Lineal**: ranuras de ventilación, divisiones de organizador. **Circular**: agujeros de una brida, dientes. **Por curva**: textura a lo largo de un borde. Ventaja: cambias la cantidad en un solo parámetro.

