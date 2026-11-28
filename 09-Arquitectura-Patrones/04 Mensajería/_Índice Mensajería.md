---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Arquitectura y patrones
tags: [indice, arquitectura-patrones]
contenido: completo
---
# Mensajería

Parte de: [[09-Arquitectura-Patrones/_Índice Arquitectura y patrones|Arquitectura y patrones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Garantías de entrega (at-most, at-least y exactly-once)]]
  - [[Entrega at-most-once]]
  - [[Entrega at-least-once]]
  - [[Entrega exactly-once (efectiva)]]
- [[Dead letter queues y mensajes venenosos]]
- [[Reintentos con backoff]]
- [[Consumidores idempotentes]]
- [[RabbitMQ]]
- [[Kafka]]

## Laboratorios

- [[Lab 3 - Outbox y consumidor idempotente]]

## Progreso

```base
filters:
  and:
    - file.inFolder("09-Arquitectura-Patrones/04 Mensajería")
    - file.name != "_Índice Mensajería"
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
