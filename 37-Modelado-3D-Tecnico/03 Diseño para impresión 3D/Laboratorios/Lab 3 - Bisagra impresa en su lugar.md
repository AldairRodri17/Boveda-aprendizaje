---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Modelado 3D técnico
herramienta: OpenSCAD o FreeCAD (gratuitos) + Elegoo Centauri 2 Combo con ElegooSlicer
tags: [modelado-3d-tecnico, laboratorio]
contenido: completo
---
# Lab 3 - Bisagra impresa en su lugar

Parte de: [[37-Modelado-3D-Tecnico/03 Diseño para impresión 3D/_Índice Diseño para impresión 3D|Diseño para impresión 3D]]

**Cubre:** [[Clips, bisagras impresas y piezas en varias partes]] · [[Holgura para piezas impresas en su lugar (print-in-place)]] · [[Ajuste deslizante y giratorio en piezas impresas]] · [[Tolerancias y holguras para piezas que encajan]] · [[Ensambles y uniones en CAD]] · [[OpenSCAD]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Diseñar una bisagra de 5 nudillos que salga de la impresora **ya armada** y gire 180°, encontrando la holgura mínima que funciona en tu impresora.

## Pasos

1. Define parámetros: largo total 40 mm, ancho de hoja 20, grosor 3, nudillo Ø8, perno Ø4, 5 nudillos, `holgura` 0.4 (radial y axial).
2. El eje de la bisagra va **paralelo a la cama** a la altura del radio del nudillo; las dos hojas quedan planas sobre la cama, una a cada lado.
3. Los nudillos pares pertenecen a la hoja A, que lleva además un **perno** continuo de Ø4 a todo lo largo. Los nudillos impares pertenecen a la hoja B y llevan un agujero de Ø4 + 2·holgura.
4. Separa los nudillos con un hueco axial igual a la holgura, y separa cada hoja de los nudillos ajenos con el mismo hueco.
5. Revisa en una vista de sección (o con un ensamble en FreeCAD) que **ningún** volumen de A toque a B.
6. Genera 3 versiones: holgura 0.3, 0.4 y 0.5. Imprímelas juntas con 3 perímetros y sin soportes.
7. Intenta girar cada una; si está pegada, aplica un giro firme una vez y anota si se liberó.

## Criterios de terminado

- [ ] La sección muestra un hueco continuo entre perno y nudillos de la otra hoja.
- [ ] Al menos una de las 3 versiones gira 180° sin herramientas.
- [ ] Anoté la holgura mínima que funciona y la guardé como `holgura_pip` en mi nota de perfil.
- [ ] La bisagra que funciona aguanta 20 ciclos de abrir/cerrar sin romperse.

## Solución

> [!success]- Código de referencia en OpenSCAD
> ```openscad
> largo = 40; ancho = 20; grosor = 3;
> d_nudillo = 8; d_perno = 4;
> holgura = 0.4;   // prueba 0.3, 0.4 y 0.5
> n = 5;           // impar: los extremos son de la hoja A
> $fn = 64; e = 0.01;
> seg = (largo - (n - 1) * holgura) / n;
> zc = d_nudillo / 2;   // altura del eje
>
> module nudillo(i)
>   translate([i * (seg + holgura), 0, zc]) rotate([0, 90, 0])
>     cylinder(d = d_nudillo, h = seg);
>
> module hoja(lado, pares) {   // lado -1 = A, +1 = B
>   r = d_nudillo / 2 + holgura;
>   translate([0, lado < 0 ? -ancho : r, 0]) cube([largo, ancho - r, grosor]);
>   for (i = [0 : n - 1]) if ((i % 2 == 0) == pares) {
>     nudillo(i);
>     translate([i * (seg + holgura), lado < 0 ? -r - e : 0, 0])
>       cube([seg, r + e, grosor]);
>   }
> }
>
> // Hoja A con perno continuo
> union() {
>   hoja(-1, true);
>   translate([0, 0, zc]) rotate([0, 90, 0]) cylinder(d = d_perno, h = largo);
> }
> // Hoja B con agujeros
> difference() {
>   hoja(1, false);
>   translate([-e, 0, zc]) rotate([0, 90, 0]) cylinder(d = d_perno + 2 * holgura, h = largo + 2 * e);
> }
> ```
> Con n = 5 y holgura 0.4 cada nudillo mide (40 − 1.6)/5 = **7.68 mm**. Revisado a mano; no se ejecutó en este equipo.

> [!success]- Si ninguna versión gira
> Casi siempre es la primera capa: baja la altura del eje para que los nudillos no apoyen (o agrega un hueco de 0.4 mm bajo los nudillos ajenos), revisa la compensación de pata de elefante y la retracción. Si el techo del agujero se cuelga, usa agujero en gota.

> [!success]- Hacerlo en FreeCAD
> Part Design: un Body por hoja; perno como *Pad* de un círculo en el plano lateral; agujeros con *Pocket*; holgura en una Spreadsheet. Verifica con *Part → Check geometry* y una sección.

## Notas personales

_Qué me costó, errores que cometí, dudas._
