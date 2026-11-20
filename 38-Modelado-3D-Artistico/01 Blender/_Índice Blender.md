---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Modelado 3D artístico
tags: [indice, modelado-3d-artistico]
---
# Blender

Parte de: [[38-Modelado-3D-Artistico/_Índice Modelado 3D artístico|Modelado 3D artístico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Blender - interfaz, navegación y modos]]
- [[Modelado poligonal en Blender]]
- [[Modificadores en Blender]]
  - [[Modificador Subdivision Surface en Blender]]
  - [[Modificador Mirror en Blender]]
  - [[Modificador Solidify en Blender]]
  - [[Modificador Boolean en Blender]]
- [[Escultura digital en Blender]]
  - [[Pinceles básicos de escultura en Blender]]
  - [[Remesh y Dyntopo en Blender]]
  - [[Multirresolución en Blender]]
- [[Retopología básica]]
- [[Preparar un modelo de Blender para imprimir]]

## Laboratorios

- [[Lab 1 - Maceta con modelado poligonal en Blender]]
- [[Lab 2 - Busto estilizado original esculpido con mouse]]

## Progreso

```base
filters:
  and:
    - file.inFolder("38-Modelado-3D-Artistico/01 Blender")
    - file.name != "_Índice Blender"
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
