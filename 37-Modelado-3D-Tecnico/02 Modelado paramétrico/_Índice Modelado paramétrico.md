---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Modelado 3D técnico
tags: [indice, modelado-3d-tecnico]
---
# Modelado paramétrico

Parte de: [[37-Modelado-3D-Tecnico/_Índice Modelado 3D técnico|Modelado 3D técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Bocetos, restricciones y cotas]]
- [[Extruir, revolucionar, barrer y recubrir]]
- [[Chaflanes, redondeos, vaciado y patrones]]
- [[Parámetros y diseño por variables]]
- [[Ensambles y uniones en CAD]]

## Laboratorios

- [[Lab 1 - Caja con tapa a presión en Onshape]]
- [[Lab 2 - Organizador paramétrico en OpenSCAD]]

## Progreso

```base
filters:
  and:
    - file.inFolder("37-Modelado-3D-Tecnico/02 Modelado paramétrico")
    - file.name != "_Índice Modelado paramétrico"
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
