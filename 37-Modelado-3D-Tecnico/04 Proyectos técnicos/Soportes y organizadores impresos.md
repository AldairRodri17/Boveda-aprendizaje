---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Modelado 3D técnico
tags: [modelado-3d-tecnico]
---
# Soportes y organizadores impresos

Parte de: [[37-Modelado-3D-Tecnico/04 Proyectos técnicos/_Índice Proyectos técnicos|Proyectos técnicos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Soportes** son piezas que sostienen algo en un lugar: ganchos, brackets de pared, bases para controles, soportes de audífonos, porta-cables, montajes bajo el escritorio. **Organizadores** son piezas que dividen un espacio: separadores de cajón, cajitas apilables, porta-tornillos, bandejas para tarjetas. Son los proyectos más frecuentes de un maker y el mejor lugar para aplicar diseño paramétrico.

### Cómo funciona
**Antes de diseñar un soporte, decide**:

1. **Carga**: cuánto pesa lo que sostiene y en qué dirección empuja (hacia abajo, hacia afuera, con palanca).
2. **Fijación**: tornillo con taquete, cinta doble cara, a presión, abrazadera.
3. **Orientación de impresión**: para que la carga corra a lo largo de las capas (ver [[Orientación de la pieza y resistencia por capas]]).
4. **Medidas críticas vs. libres**: las que encajan con algo (ancho del objeto, separación de tornillos) llevan holgura y se miden bien; las demás son estéticas.

```mermaid
flowchart LR
  A[Carga y dirección] --> B[Fijación]
  B --> C[Orientación]
  C --> D[Medidas críticas con holgura]
  D --> E[Diseño paramétrico]
  E --> F[Prueba con el doble de carga]
```

**Organizadores paramétricos**: los cajones, las cajas y los objetos cambian. Si el organizador depende de variables (largo, ancho, alto, número de divisiones, radio interior), generas una versión nueva en segundos sin rediseñar. En [[OpenSCAD]] además puedes compartir el `.scad` y cualquiera lo ajusta en el Customizer. Sistemas modulares como las rejillas de base común (tipo "gridfinity") se basan en este principio: una unidad fija y piezas múltiplos de ella.

**Ergonomía**: esquinas interiores **redondeadas** en el fondo para que el dedo saque los tornillos; paredes de 2–4 líneas; bordes superiores con un pequeño redondeo para que no rasquen.

### Ejemplo
Organizador paramétrico de cajón con fondo redondeado:

```openscad
largo = 120;  ancho = 80;  alto = 35;   // [Customizer]
n = 3;                                   // compartimentos
r = 6;                                   // radio interior del fondo
pared = 1.68;  div = 1.26;  piso = 1.2;

w = (largo - 2*pared - (n - 1)*div) / n;   // ancho de cada compartimento

module hueco(w, a, h) {
  // caja con aristas inferiores redondeadas
  hull() {
    for (x = [r, w - r], y = [r, a - r]) translate([x, y, r]) sphere(r = r, $fn = 32);
    translate([0, 0, r]) cube([w, a, h]);
  }
}
difference() {
  cube([largo, ancho, alto]);
  for (k = [0 : n - 1])
    translate([pared + k * (w + div), pared, piso]) hueco(w, ancho - 2*pared, alto);
}
echo(w = w);   // 38.04 mm con los valores por defecto
```

`hull()` de cuatro esferas y una caja da un hueco con el fondo redondeado. El `echo` te dice el ancho real de cada compartimento.

### Práctica en tu Centauri 2
Mide un cajón real de tu escritorio y genera tres organizadores distintos con el mismo `.scad` cambiando solo las variables. Imprime uno en PLA con 2 perímetros y 10 % de relleno (no carga peso). Después diseña un soporte bajo el escritorio para tu hub o la base del [[DJI Mic Mini 2]] y pruébalo con el doble de peso durante un día. Lab relacionado: [[Lab 2 - Organizador paramétrico en OpenSCAD]].

### Cuándo usarlo y cuándo no
Organizadores: casi siempre vale la pena imprimirlos a medida. Soportes: sí para cargas ligeras y medianas en interiores. Para cargas pesadas (repisas con libros, una TV, cosas sobre la cabeza de alguien), usa herrajes metálicos comerciales.

### Errores comunes
- **Esquinas interiores a 90°**: no entra el dedo y se acumula polvo; redondea el fondo.
- **Paredes muy delgadas**: se rompen al sacar el organizador; 2–4 líneas.
- **No considerar la holgura del cajón**: el organizador queda justo y no sale; resta 0.5–1 mm al largo y ancho del cajón.
- **Soporte orientado con la carga separando capas**: se rompe en la esquina.

### Para profundizar
- [Documentación oficial de OpenSCAD](https://openscad.org/documentation.html)
- [hull() y transformaciones en el manual de OpenSCAD](https://en.wikibooks.org/wiki/OpenSCAD_User_Manual/Transformations)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué decides antes de diseñar un soporte?
> Qué carga lleva y en qué dirección, cómo se fija (tornillo, adhesivo, a presión), en qué orientación se imprime y qué medidas son críticas (las que encajan) frente a las libres.

> [!question]- ¿Por qué hacer un organizador paramétrico y no uno fijo?
> Porque los cajones y objetos cambian: con variables de largo, ancho, alto y número de divisiones generas versiones nuevas sin rediseñar. Puedes compartir el `.scad` y cada quien lo ajusta en el Customizer.

> [!question]- Error común en organizadores. ¿Cuál?
> Esquinas interiores a 90° donde no entra el dedo y paredes demasiado delgadas. Redondea el fondo y diseña paredes de 2–4 líneas.

## Práctica

- [[Lab 2 - Organizador paramétrico en OpenSCAD]]
- [[Lab 4 - Soporte de iPhone para tripié con tuerca atrapada]]

