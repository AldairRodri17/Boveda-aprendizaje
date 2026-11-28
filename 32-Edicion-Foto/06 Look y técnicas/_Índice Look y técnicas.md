---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de fotografía
tags: [indice, edicion-foto]
contenido: completo
---
# Look y técnicas

Parte de: [[32-Edicion-Foto/_Índice Edición de fotografía|Edición de fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Presets y LUTs en fotografía]]
- [[Consistencia de color en una serie]]
- [[Blanco y negro]]
- [[Unir HDR y panorámicas]]

## Laboratorios

- [[Práctica Edición 08 - Blanco y negro con intención]]
- [[Práctica Edición 09 - Recrear un look y aplicarlo a una serie]]

## Progreso

```base
filters:
  and:
    - file.inFolder("32-Edicion-Foto/06 Look y técnicas")
    - file.name != "_Índice Look y técnicas"
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
