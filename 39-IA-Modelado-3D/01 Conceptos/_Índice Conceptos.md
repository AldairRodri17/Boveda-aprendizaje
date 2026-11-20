---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA para modelado 3D
tags: [indice, ia-modelado-3d]
---
# Conceptos

Parte de: [[39-IA-Modelado-3D/_Índice IA para modelado 3D|IA para modelado 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Texto a 3D e imagen a 3D]]
- [[Representaciones 3D (malla, NeRF, Gaussian splatting y campos implícitos)]]
  - [[Mallas poligonales en IA 3D]]
  - [[NeRF (campos de radiancia neuronal)]]
  - [[Gaussian splatting en IA 3D]]
  - [[Campos implícitos (SDF y ocupación)]]
- [[Cómo funciona un modelo de imagen a 3D]]
- [[Servicios comerciales de IA 3D (Tripo y similares)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("39-IA-Modelado-3D/01 Conceptos")
    - file.name != "_Índice Conceptos"
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
