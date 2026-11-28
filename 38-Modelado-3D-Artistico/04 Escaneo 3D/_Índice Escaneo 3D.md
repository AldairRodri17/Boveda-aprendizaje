---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Modelado 3D artístico
tags: [indice, modelado-3d-artistico]
contenido: completo
---
# Escaneo 3D

Parte de: [[38-Modelado-3D-Artistico/_Índice Modelado 3D artístico|Modelado 3D artístico]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Fotogrametría y escaneo con iPhone]]
- [[Limpiar un escaneo y hacerlo imprimible]]

## Laboratorios

- [[Lab 7 - Escanear un objeto con el iPhone y dejarlo imprimible]]

## Progreso

```base
filters:
  and:
    - file.inFolder("38-Modelado-3D-Artistico/04 Escaneo 3D")
    - file.name != "_Índice Escaneo 3D"
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
