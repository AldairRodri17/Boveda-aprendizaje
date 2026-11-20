---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Astronomía y astrofotografía
tags: [indice, astronomia]
---
# Planeación

Parte de: [[64-Astronomia/_Índice Astronomía y astrofotografía|Astronomía y astrofotografía]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Planear una salida de astrofotografía (Luna, clima y lugar)]] ⭐
- [[Seguridad y equipo para salir de noche]]
- [[Astrofotografía con dron (qué sí y qué no se puede)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("64-Astronomia/06 Planeación")
    - file.name != "_Índice Planeación"
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
