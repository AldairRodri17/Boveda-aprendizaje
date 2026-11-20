---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Modelado 3D artístico
herramienta: Blender (gratuito) + Elegoo Centauri 2 Combo con ElegooSlicer
tags: [modelado-3d-artistico, laboratorio]
---
# Lab 1 - Maceta con modelado poligonal en Blender

Parte de: [[38-Modelado-3D-Artistico/01 Blender/_Índice Blender|Blender]]

**Cubre:** [[Blender - interfaz, navegación y modos]] · [[Modelado poligonal en Blender]] · [[Modificadores en Blender]] · [[Modificador Subdivision Surface en Blender]] · [[Modificador Solidify en Blender]] · [[Macetas, lámparas y objetos decorativos]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Modelar en Blender, con modelado poligonal y modificadores, una maceta de unos 80 mm de diámetro con agujero de drenaje, e imprimirla.

## Pasos

1. Configura unidades: *Metric*, *Unit Scale* 0.001, *Length* en milímetros. Activa *Emulate Numpad* si no tienes teclado numérico.
2. Borra el cubo y agrega un cilindro de 32 lados, radio 40 mm y altura 75 mm. Aplica escala (`Ctrl+A`).
3. Edit Mode: borra la cara superior. Con `Ctrl+R` agrega 2–3 bucles y escala algunos para dar forma (más ancha arriba, un bordo en el labio).
4. Selecciona la cara de abajo, `I` (inset) de 12 mm y borra la cara central para el drenaje (o hazlo después con Boolean).
5. Agrega **Solidify** (2 mm, Offset −1) y luego **Subdivision Surface** nivel 2. Agrega bucles de apoyo en el borde superior y en la base para que no se redondeen de más.
6. Activa el *overlay* Face Orientation y corrige normales si ves rojo afuera.
7. Aplica los modificadores, exporta STL y lamina en tu [[Elegoo Centauri 2 Combo]] con [[ElegooSlicer y OrcaSlicer]] con 3 perímetros y 15 % de relleno.
8. Imprime y prueba con agua sobre un plato.

## Criterios de terminado

- [ ] Las dimensiones en el laminador son ~80 mm de diámetro y ~75 mm de alto.
- [ ] La pared mide 2 mm (la medí en el corte de la vista previa del laminador).
- [ ] La base es plana y el agujero de drenaje existe.
- [ ] La pieza se imprimió sin soportes.

## Solución

> [!success]- Si Subdivision deforma la base
> Agrega un bucle de apoyo muy cerca del borde inferior (`Ctrl+R` y desliza) o marca el borde con *Edge Crease* (`Shift+E`, valor 1).

> [!success]- Por qué el orden Solidify → Subdivision
> Primero das grosor y después suavizas ambas caras juntas; al revés, la subdivisión suaviza una superficie delgada y Solidify crea paredes con grosor irregular en zonas curvas.

## Notas personales

_Qué me costó, errores que cometí, dudas._
