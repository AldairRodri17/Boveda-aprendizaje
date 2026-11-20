---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fotografía
tags: [indice, fotografia]
---
# Composición

Parte de: [[30-Fotografia/_Índice Fotografía|Fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Regla de tercios y proporción áurea]]
- [[Líneas guía, encuadre natural, simetría y patrones]]
  - [[Líneas guía en composición]]
  - [[Encuadre natural en composición]]
  - [[Simetría en composición]]
  - [[Patrones y repetición en composición]]
- [[Espacio negativo y simplicidad]]
- [[Punto de vista y capas de profundidad]]
- [[Educar el ojo (analizar fotos y fotógrafos)]]

## Laboratorios

- [[Práctica Foto 07 - Serie de 9 fotos de composición]]

## Progreso

```base
filters:
  and:
    - file.inFolder("30-Fotografia/05 Composición")
    - file.name != "_Índice Composición"
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
