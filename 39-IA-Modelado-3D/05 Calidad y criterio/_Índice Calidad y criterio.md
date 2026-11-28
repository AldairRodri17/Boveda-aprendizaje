---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: IA para modelado 3D
tags: [indice, ia-modelado-3d]
contenido: completo
---
# Calidad y criterio

Parte de: [[39-IA-Modelado-3D/_Índice IA para modelado 3D|IA para modelado 3D]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué imágenes funcionan mejor para IA 3D]]
- [[Errores típicos de los modelos generados por IA]]
- [[IA vs CAD vs escaneo (cuándo usar cada uno)]]

## Laboratorios

- [[Lab 7 - Escaneo con iPhone vs IA del mismo objeto]]

## Progreso

```base
filters:
  and:
    - file.inFolder("39-IA-Modelado-3D/05 Calidad y criterio")
    - file.name != "_Índice Calidad y criterio"
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
