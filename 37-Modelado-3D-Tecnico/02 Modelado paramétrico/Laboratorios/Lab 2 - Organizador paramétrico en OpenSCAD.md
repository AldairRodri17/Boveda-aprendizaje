---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Modelado 3D técnico
herramienta: OpenSCAD (gratuito) + Elegoo Centauri 2 Combo con ElegooSlicer
tags: [modelado-3d-tecnico, laboratorio]
---
# Lab 2 - Organizador paramétrico en OpenSCAD

Parte de: [[37-Modelado-3D-Tecnico/02 Modelado paramétrico/_Índice Modelado paramétrico|Modelado paramétrico]]

**Cubre:** [[OpenSCAD]] · [[Parámetros y diseño por variables]] · [[Soportes y organizadores impresos]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Escribir un organizador de cajón **totalmente paramétrico** en OpenSCAD (medidas, pared, número de divisiones en X y Y, radio de esquinas) usable desde el Customizer, e imprimir una versión que quepa en tu cajón.

## Pasos

1. Mide el hueco del cajón con regla (largo, ancho, alto útil) y réstale 1 mm por lado de holgura.
2. Declara las variables del Customizer al inicio del archivo con sus rangos (`// [min:paso:max]`): `largo`, `ancho`, `alto`, `pared`, `piso`, `div_x`, `div_y`, `div_grosor`, `radio`.
3. Escribe un módulo `caja_redondeada(l, a, h, r)` con `hull()` de cuatro cilindros (y un `cube` si `r <= 0`).
4. Escribe el módulo `organizador(...)`: calcula el ancho de cada compartimento, valida con `assert` que no quede menor a 5 mm, y resta los compartimentos con un `for` de dos variables.
5. Agrega un segundo `assert` para que el radio exterior no deje la esquina más delgada que 0.8 mm (pista: la pared en la diagonal de la esquina es `r - sqrt(2)*(r - pared)`).
6. Abre el Customizer, prueba 3 combinaciones (2×1, 3×2, 5×3) y revisa que todo regenere.
7. Renderiza (`F6`), exporta 3MF o STL y lamina con 2–3 perímetros. Imprime primero una versión baja (`alto = 10`) para validar que cabe.

## Criterios de terminado

- [ ] Al cambiar `div_x` o `div_y` en el Customizer el modelo se regenera sin errores.
- [ ] Con `div_x = 30` el `assert` detiene el render con un mensaje claro.
- [ ] La versión de prueba de 10 mm entra en el cajón con 0.5–1 mm de juego por lado.
- [ ] El organizador final se imprimió sin soportes y las divisiones salieron sólidas (sin huecos entre líneas).
- [ ] El archivo `.scad` está guardado (idealmente en un repositorio Git) con un comentario de cómo usarlo.

## Solución

> [!success]- Código de referencia
> ```openscad
> // Organizador paramétrico
> largo = 120;      // [40:5:250]
> ancho = 80;       // [30:5:200]
> alto = 40;        // [10:5:100]
> pared = 1.68;     // [0.84:0.42:4.2]
> piso = 1.2;       // [0.6:0.2:3]
> div_x = 3;        // [1:1:10]
> div_y = 2;        // [1:1:10]
> div_grosor = 1.26; // [0.84:0.42:3]
> radio = 3;        // [0:0.5:10]
>
> $fn = 48;
>
> module caja_redondeada(l, a, h, r) {
>   if (r <= 0) cube([l, a, h]);
>   else hull() for (x = [r, l - r], y = [r, a - r])
>     translate([x, y, 0]) cylinder(r = r, h = h);
> }
>
> module organizador(l, a, h, p, f, nx, ny, g, r) {
>   cx = (l - 2*p - (nx - 1)*g) / nx;
>   cy = (a - 2*p - (ny - 1)*g) / ny;
>   assert(cx >= 5 && cy >= 5, str("Compartimentos muy chicos: ", cx, " x ", cy));
>   assert(r <= p || r - sqrt(2)*(r - p) >= 0.8, "El radio deja la esquina muy delgada");
>   difference() {
>     caja_redondeada(l, a, h, r);
>     for (i = [0 : nx - 1], j = [0 : ny - 1])
>       translate([p + i*(cx + g), p + j*(cy + g), f])
>         cube([cx, cy, h]);   // sobresale por arriba a propósito
>   }
> }
>
> organizador(largo, ancho, alto, pared, piso, div_x, div_y, div_grosor, radio);
> ```
> Con los valores de ejemplo cada compartimento mide (120 − 3.36 − 2.52)/3 = **38.04 × 37.69 mm**. El código está revisado a mano; si tu versión de OpenSCAD marca algo, lee el mensaje de la consola: casi siempre es un `;` o un paréntesis.

> [!success]- Por qué el radio tiene límite
> La esquina interior del compartimento es recta y la exterior es un arco: el punto más delgado está en la diagonal y mide `r − √2·(r − pared)`. Con pared 1.68 y r = 3 quedan ≈ 1.13 mm; con r = 5 quedarían 0.30 mm (frágil). Una mejora: redondear también los compartimentos con `offset()` o con otro `hull()`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
