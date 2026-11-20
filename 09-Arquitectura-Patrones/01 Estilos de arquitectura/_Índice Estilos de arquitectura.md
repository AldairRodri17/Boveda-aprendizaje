---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Arquitectura y patrones
tags: [indice, arquitectura-patrones]
---
# Estilos de arquitectura

Parte de: [[09-Arquitectura-Patrones/_Índice Arquitectura y patrones|Arquitectura y patrones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Arquitectura en capas]]
- [[Vertical Slice Architecture]]
- [[Arquitectura hexagonal]]
- [[Clean Architecture]]
- [[Monolito modular]]
- [[Microservicios]]
- [[Arquitectura orientada a eventos]]
- [[CQRS y mediador]]
- [[Event Sourcing]]
- [[Domain-Driven Design - conceptos]]
- [[Entidades, value objects y agregados]]
- [[Bounded contexts]]
- [[Eventos de dominio y reacciones]]
- [[Arquitectura evolutiva]]
- [[Atributos de calidad y trade-offs]]

## Progreso

```base
filters:
  and:
    - file.inFolder("09-Arquitectura-Patrones/01 Estilos de arquitectura")
    - file.name != "_Índice Estilos de arquitectura"
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
