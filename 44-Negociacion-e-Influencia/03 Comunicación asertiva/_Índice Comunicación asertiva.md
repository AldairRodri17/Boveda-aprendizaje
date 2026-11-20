---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Negociación e influencia
tags: [indice, negociacion]
---
# Comunicación asertiva

Parte de: [[44-Negociacion-e-Influencia/_Índice Negociación e influencia|Negociación e influencia]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Comunicación asertiva]] ⭐
- [[Conversaciones difíciles]] ⭐
- [[Poner límites]]
- [[Pedir y dar disculpas]]

## Laboratorios

- [[Lab 2 - Ensayar una conversación difícil]]

## Progreso

```base
filters:
  and:
    - file.inFolder("44-Negociacion-e-Influencia/03 Comunicación asertiva")
    - file.name != "_Índice Comunicación asertiva"
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
