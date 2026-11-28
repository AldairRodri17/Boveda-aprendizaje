---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Modelado 3D técnico
herramienta: Onshape (plan gratuito, documentos públicos) + Elegoo Centauri 2 Combo con ElegooSlicer
tags: [modelado-3d-tecnico, laboratorio]
contenido: completo
---
# Lab 1 - Caja con tapa a presión en Onshape

Parte de: [[37-Modelado-3D-Tecnico/02 Modelado paramétrico/_Índice Modelado paramétrico|Modelado paramétrico]]

**Cubre:** [[Onshape (plan gratuito)]] · [[Bocetos, restricciones y cotas]] · [[Extruir, revolucionar, barrer y recubrir]] · [[Tolerancias y holguras para piezas que encajan]] · [[Ajuste a presión en piezas impresas]] · [[Cajas para electrónica]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Diseñar en Onshape una caja con tapa que cierre **a presión**, controlada por variables, e imprimirla hasta que la tapa no se caiga con la caja de cabeza.

> [!warning] Documentos públicos
> En el plan gratuito todo lo que creas es público. Ponle un nombre genérico y no incluyas nada personal.

## Pasos

1. Crea un documento en Onshape y define con la operación *Variable*: `#largo = 80`, `#ancho = 60`, `#alto = 35`, `#pared = 1.68`, `#piso = 1.2`, `#holgura = 0.15`, `#labio = 5` (mm).
2. Boceto en el plano Top: rectángulo **simétrico al origen** de `#largo × #ancho`, totalmente restringido (todo negro).
3. Extruye `#alto`. Aplica **Shell** quitando la cara superior con grosor `#pared`. Agrega chaflán de 0.5 mm al borde inferior exterior.
4. Tapa: nuevo boceto en Top, mismo rectángulo, extrusión de 2 mm como **pieza nueva** (*New*). Sobre su cara, boceto del labio: rectángulo de `#largo - 2*#pared - 2*#holgura` por `#ancho - 2*#pared - 2*#holgura` con pared de 1.26 mm, extruido `#labio`.
5. Opcional: un cordón (*retén*) de 0.3 mm de alto alrededor del labio a 2 mm de la punta y una ranura correspondiente en la caja.
6. Antes de imprimir todo, haz una **prueba recortada**: copia la caja y la tapa, córtalas a 8 mm de alto con *Split* o una extrusión de remoción y exporta solo eso.
7. Exporta a STL o 3MF e imprime en tu [[Elegoo Centauri 2 Combo]] y [[ElegooSlicer y OrcaSlicer]]: caja con el piso en la cama; tapa con la cara exterior en la cama (labio hacia arriba).
8. Prueba el cierre. Ajusta `#holgura` en pasos de 0.05 hasta que cierre firme; luego imprime las piezas completas.

## Criterios de terminado

- [ ] Cambiar `#largo` a 100 regenera caja y tapa sin errores y la tapa sigue ajustando.
- [ ] El boceto base está totalmente restringido.
- [ ] Volteo la caja cerrada con un objeto de ~100 g adentro y la tapa no se cae.
- [ ] La tapa se quita con la mano sin herramientas ni grietas.
- [ ] Anoté en la nota la `#holgura` final y el filamento usado.

## Solución

> [!success]- Valores que suelen funcionar (a calibrar)
> Con PLA y boquilla de 0.4 mm, el ajuste firme suele quedar entre **0.10 y 0.20 mm por lado**. Si la parte baja del labio entra apretada y la de arriba no, es pata de elefante: agrega un chaflán de 0.4 mm a la punta del labio.

> [!success]- Por qué la tapa se imprime boca abajo
> La cara exterior queda lisa contra la cama y el labio no necesita soportes. Si la imprimieras con el labio abajo, el borde del labio sería la primera capa y saldría más ancho.

## Notas personales

_Qué me costó, errores que cometí, dudas._
