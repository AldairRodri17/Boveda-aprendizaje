---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Arquitectura y patrones
tags: [indice, arquitectura-patrones]
---
# Patrones de nube y sistemas distribuidos

Parte de: [[09-Arquitectura-Patrones/_Índice Arquitectura y patrones|Arquitectura y patrones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Patrón Saga]]
- [[Circuit breaker y reintentos]]
- [[Bulkhead y throttling]]
- [[Cache-aside y estrategias de caché]]
- [[Competing consumers]]
- [[Valet key (URLs firmadas)]]
- [[Strangler fig]]
- [[Anti-corruption layer]]
- [[Backend for Frontend (BFF)]]
- [[Sidecar]]
- [[Consistencia eventual y teorema CAP]]
- [[Modelos de consistencia (fuerte, eventual, read-your-writes)]]
- Idempotencia distribuida: ver [[Idempotencia]]
- [[Multi-tenancy]]
- [[Escalabilidad horizontal y vertical]]
- [[Las doce factores (12-factor)]]

## Laboratorios

- [[Lab 2 - Resiliencia con reintentos y circuit breaker en .NET]]

## Progreso

```base
filters:
  and:
    - file.inFolder("09-Arquitectura-Patrones/03 Patrones de nube y sistemas distribuidos")
    - file.name != "_Índice Patrones de nube y sistemas distribuidos"
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
