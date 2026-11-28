---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Impresión 3D
tags: [indice, impresion-3d]
contenido: completo
---
# Comunidad, licencias y costos

Parte de: [[36-Impresion-3D/_Índice Impresión 3D|Impresión 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Repositorios de modelos (Printables, MakerWorld y Thingiverse)]]
- [[Licencias de modelos 3D (Creative Commons y uso comercial)]]
- [[Personajes y marcas registradas en impresión 3D]]
- [[Costeo de una impresión 3D]]

## Progreso

```base
filters:
  and:
    - file.inFolder("36-Impresion-3D/07 Comunidad, licencias y costos")
    - file.name != "_Índice Comunidad, licencias y costos"
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
