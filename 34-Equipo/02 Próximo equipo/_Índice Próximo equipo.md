---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Equipo
tags: [indice, equipo]
contenido: completo
---
# Próximo equipo

Parte de: [[34-Equipo/_Índice Equipo|Equipo]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[DJI Osmo Pocket 4P]]
- [[Sony a6700]]
- [[Lentes para la Sony a6700]]
- [[Sony S-Cinetone y S-Log3]]

## Laboratorios

- [[Lab 3 - Primer día con la Sony a6700 (perfiles, LUT y tarjetas)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("34-Equipo/02 Próximo equipo")
    - file.name != "_Índice Próximo equipo"
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
