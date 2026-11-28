---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Angular
tags: [indice, angular]
contenido: completo
---
# Básico

Parte de: [[20-Angular/_Índice Angular|Angular]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es Angular y en qué se diferencia de React]]
- [[Angular CLI y estructura de un proyecto]]
- [[TypeScript para Angular]]
- [[Componentes en Angular]]
- [[Componentes standalone]]
- [[Templates y data binding (interpolación, propiedades y eventos)]]
- [[Two-way binding]]
- [[Control flow (@if, @for y @switch)]]
- [[Input y Output]]
- [[Ciclo de vida de componentes]]
- [[Directivas]]
- [[Pipes]]

## Laboratorios

- [[Lab 1 - Componentes standalone y data binding en Angular]]

## Progreso

```base
filters:
  and:
    - file.inFolder("20-Angular/01 Básico")
    - file.name != "_Índice Básico"
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
