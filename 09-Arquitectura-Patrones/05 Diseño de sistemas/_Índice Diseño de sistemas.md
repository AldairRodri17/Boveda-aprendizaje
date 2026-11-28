---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Arquitectura y patrones
tags: [indice, arquitectura-patrones]
contenido: completo
---
# Diseño de sistemas

Parte de: [[09-Arquitectura-Patrones/_Índice Arquitectura y patrones|Arquitectura y patrones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Diseño de sistemas - cómo abordarlo]]
- [[Requisitos funcionales y no funcionales]]
- [[Estimaciones rápidas (carga, almacenamiento y costo)]]
- [[Replicación y réplicas de lectura]]
- [[Sharding y particionamiento]]
- [[Locks distribuidos y relojes]]
- [[Backpressure]]
- [[Caso - acortador de URLs]]
- [[Caso - rate limiter]]
- [[Caso - sistema de notificaciones]]
- [[Caso - feed de noticias]]
- [[Caso - sistema de pagos]]
- [[Caso - chat en tiempo real]]
- [[Caso - subida y procesamiento de archivos]]

## Laboratorios

- [[Lab 4 - Diseño e implementación de un rate limiter]]

## Progreso

```base
filters:
  and:
    - file.inFolder("09-Arquitectura-Patrones/05 Diseño de sistemas")
    - file.name != "_Índice Diseño de sistemas"
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
