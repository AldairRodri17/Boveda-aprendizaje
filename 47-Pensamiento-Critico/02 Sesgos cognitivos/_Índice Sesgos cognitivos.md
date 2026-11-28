---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Pensamiento crítico y toma de decisiones
tags: [indice, pensamiento-critico]
contenido: completo
---
# Sesgos cognitivos

Parte de: [[47-Pensamiento-Critico/_Índice Pensamiento crítico y toma de decisiones|Pensamiento crítico y toma de decisiones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Sesgos cognitivos]] ⭐ (nota índice)
- [[Sesgo de confirmación]]
- [[Sesgo de anclaje]]
- [[Falacia del costo hundido]] ⭐
- [[Exceso de confianza y efecto Dunning-Kruger]]

## Progreso

```base
filters:
  and:
    - file.inFolder("47-Pensamiento-Critico/02 Sesgos cognitivos")
    - file.name != "_Índice Sesgos cognitivos"
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
