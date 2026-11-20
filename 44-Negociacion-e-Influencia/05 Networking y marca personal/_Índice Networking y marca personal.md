---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Negociación e influencia
tags: [indice, negociacion]
---
# Networking y marca personal

Parte de: [[44-Negociacion-e-Influencia/_Índice Negociación e influencia|Negociación e influencia]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Networking sin incomodidad]] ⭐
- [[LinkedIn para desarrolladores]] ⭐
- [[Crear contenido técnico (blog, videos y charlas)]]
- [[Comunidades y meetups]]

## Laboratorios

- [[Lab 3 - Mejorar mi perfil de LinkedIn]]

## Progreso

```base
filters:
  and:
    - file.inFolder("44-Negociacion-e-Influencia/05 Networking y marca personal")
    - file.name != "_Índice Networking y marca personal"
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
