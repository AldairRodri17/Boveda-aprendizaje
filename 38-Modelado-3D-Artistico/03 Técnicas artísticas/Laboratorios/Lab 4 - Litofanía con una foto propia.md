---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Modelado 3D artístico
herramienta: GIMP o Photopea (gratuitos), Blender con modificador Displace, Elegoo Centauri 2 Combo con PLA blanco
tags: [modelado-3d-artistico, laboratorio]
contenido: completo
---
# Lab 4 - Litofanía con una foto propia

Parte de: [[38-Modelado-3D-Artistico/03 Técnicas artísticas/_Índice Técnicas artísticas|Técnicas artísticas]]

**Cubre:** [[Litofanías]] · [[Modificadores en Blender]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Convertir una foto **tuya** en una litofanía de 100 × 75 mm en Blender e imprimirla.

## Pasos

1. Elige una foto propia con buen contraste. En [[GIMP]] o [[Photopea]]: recórtala a 4:3, pásala a escala de grises, ajusta curvas y **inviértela** (claros oscuros). Expórtala en PNG.
2. Blender en mm: *Add → Mesh → Grid* de tamaño 100 mm con 500 × 375 subdivisiones y escala Y a 0.75 (y aplica escala). El Grid ya trae UV.
3. Agrega **Displace**: textura de imagen con tu PNG, *Coordinates: UV*, *Direction: Z*, *Midlevel* 0 y *Strength* 2.4.
4. Aplica el modificador. Edit Mode: selecciona todo, `Shift+D` y clic derecho (duplica en el lugar), `S Z 0` para aplanar la copia y en la barra lateral (`N`) pon su Z media en −0.8 mm: es la cara trasera.
5. Selecciona todo, *Select → Select Loops → Select Boundary Loop* (toma los dos contornos) y *Edge → Bridge Edge Loops* para cerrar los costados. `Shift+N` para recalcular normales.
6. Revisa *Non Manifold* (debe dar 0) y que el grosor vaya de 0.8 a 3.2 mm.
7. Agrega un marco de 3 mm alrededor (opcional) y exporta STL.
8. Lamina parada (vertical) con brim, relleno 100 % y capa de 0.12–0.16 mm. Imprime en PLA blanco.
9. Pruébala frente a una lámpara LED o la ventana.

## Criterios de terminado

- [ ] La placa mide 100 × 75 mm con grosor entre 0.8 y 3.2 mm.
- [ ] La malla es manifold.
- [ ] Con luz detrás se reconoce la foto con claros y oscuros correctos.
- [ ] La foto es mía.

## Solución

> [!success]- Por qué invertir
> El Displace con *Midlevel* 0 sube más donde el pixel es claro. Queremos lo contrario (lo claro debe ser delgado), por eso se invierte la foto antes: los tonos oscuros de la foto original quedan altos (gruesos).

> [!success]- Si se ve pixelada o sin detalle
> Más subdivisiones del Grid (0.2 mm por vértice o menos), capa más baja y imprimir vertical para que el detalle quede en XY. Si se ve muy oscura, baja el grosor máximo a 2.8 mm.

## Notas personales

_Qué me costó, errores que cometí, dudas._
