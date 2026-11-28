---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Modelado 3D técnico
tags: [indice, modelado-3d-tecnico]
contenido: completo
---
# Programas gratuitos

Parte de: [[37-Modelado-3D-Tecnico/_Índice Modelado 3D técnico|Modelado 3D técnico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Tinkercad]]
- [[Onshape (plan gratuito)]]
- [[FreeCAD]]
- [[Fusion (licencia personal)]]
- [[OpenSCAD]]

## Progreso

```base
filters:
  and:
    - file.inFolder("37-Modelado-3D-Tecnico/01 Programas gratuitos")
    - file.name != "_Índice Programas gratuitos"
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
