---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Impresión 3D
tags: [indice, impresion-3d]
---
# Laminador

Parte de: [[36-Impresion-3D/_Índice Impresión 3D|Impresión 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[ElegooSlicer y OrcaSlicer]]
- [[Altura de capa y ancho de línea]]
- [[Perímetros y relleno]]
- [[Soportes (normales, árbol e interfaces)]]
- [[Adherencia a la cama (brim, raft y superficies)]]
- [[Velocidades, aceleraciones y enfriamiento]]
- [[Costura, ironing y altura de capa variable]]
- [[Impresión multicolor (pintar colores y torre de purga)]]
- [[Perfiles de filamento e impresora]]
- [[Archivos para imprimir (STL, 3MF, OBJ y STEP)]]

## Laboratorios

- [[Lab 5 - Pieza en 4 colores con CANVAS minimizando purga]]

## Progreso

```base
filters:
  and:
    - file.inFolder("36-Impresion-3D/02 Laminador")
    - file.name != "_Índice Laminador"
views:
  - type: table
    name: Temas
    filters:
      and:
        - tipo != "laboratorio"
    order:
      - file.name
      - progreso
      - nivel
  - type: table
    name: Laboratorios
    filters:
      and:
        - tipo == "laboratorio"
    order:
      - file.name
      - progreso
```
