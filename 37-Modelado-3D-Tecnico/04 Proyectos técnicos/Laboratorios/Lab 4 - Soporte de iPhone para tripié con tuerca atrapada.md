---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Modelado 3D técnico
herramienta: Onshape o FreeCAD + OpenSCAD opcional + Elegoo Centauri 2 Combo con ElegooSlicer; regla metálica (calibrador recomendado)
tags: [modelado-3d-tecnico, laboratorio]
---
# Lab 4 - Soporte de iPhone para tripié con tuerca atrapada

Parte de: [[37-Modelado-3D-Tecnico/04 Proyectos técnicos/_Índice Proyectos técnicos|Proyectos técnicos]]

**Cubre:** [[Accesorios impresos para foto, video y dron]] · [[Medir piezas e ingeniería inversa]] · [[Agujeros, roscas impresas, insertos y tuercas atrapadas]] · [[Orientación de la pieza y resistencia por capas]] · [[Soportes y organizadores impresos]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Diseñar un soporte que sujete tu iPhone 15 Pro (con su funda) y se monte en un tripié con tornillo 1/4"-20, usando una tuerca atrapada. Variante: clip o base para el transmisor del DJI Mic Mini 2.

> [!tip] Sin calibrador
> Usa regla metálica, mide 3 veces y redondea; para el ancho del teléfono con funda, colócalo entre dos libros paralelos y mide la distancia entre ellos. Imprime una **galga** de ranuras (p. ej. 9, 10, 11, 12 mm) para el grosor. Un [[Calibrador digital (vernier)]] te ahorra muchas impresiones de prueba.

## Pasos

1. Mide el teléfono **con la funda** (ancho y grosor) y una tuerca 1/4"-20 (entre caras). Compara con la ficha técnica oficial del teléfono para detectar errores de medición.
2. Imprime una galga para el grosor del teléfono y otra para el hexágono de la tuerca (3 tamaños cada una) y elige las que ajustan.
3. Diseña la base: bloque de al menos 15 mm de alto con agujero pasante de Ø6.8 para el tornillo y alojamiento hexagonal para la tuerca desde arriba (cara plana hacia arriba).
4. Diseña las mordazas: dos brazos en U con labio de 3 mm sobre el frente del teléfono, holgura de 0.3 mm por lado y redondeo en la raíz de cada brazo.
5. Decide la orientación: la U acostada para que los brazos se flexionen a lo largo de las capas. Si el agujero del tornillo queda horizontal, hazlo en gota.
6. Imprime en PETG con 4 perímetros. Mete la tuerca (a presión o pausa en la capa correspondiente).
7. Monta en el tripié, coloca el teléfono y prueba en vertical y horizontal.

## Criterios de terminado

- [ ] El teléfono entra y sale con una mano y no se cae al voltear el tripié 90°.
- [ ] La tuerca no gira al apretar el tornillo del tripié.
- [ ] No hay grietas en la raíz de los brazos después de 20 inserciones.
- [ ] Registré en la nota las medidas finales y la holgura usada.

## Solución

> [!success]- Medidas de partida
> La tuerca 1/4"-20 suele medir 11.11 mm (7/16") entre caras: `d = (11.11 + 0.2)/cos(30) ≈ 13.06` en OpenSCAD. El tornillo es de 6.35 mm: agujero de 6.6–6.8 para que pase libre. Las medidas del teléfono sin funda están en la ficha de Apple; con funda, manda la medida real.

> [!success]- Por qué PETG y brazos acostados
> Los brazos trabajan a flexión repetida: en PETG duran más que en PLA, y acostados la flexión va a lo largo de los filamentos en lugar de abrir capas.

> [!success]- Variante DJI Mic Mini 2
> Mide el transmisor y su clip, y diseña una base con ranura (holgura 0.2–0.3) para dejarlo de pie en el escritorio, o un adaptador a zapata fría. Mide la zapata de tu cámara o tripié en vez de suponer medidas.

## Notas personales

_Qué me costó, errores que cometí, dudas._
