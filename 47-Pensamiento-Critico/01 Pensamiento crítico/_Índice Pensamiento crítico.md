---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Pensamiento crítico y toma de decisiones
tags: [indice, pensamiento-critico]
contenido: completo
---
# Pensamiento crítico

Parte de: [[47-Pensamiento-Critico/_Índice Pensamiento crítico y toma de decisiones|Pensamiento crítico y toma de decisiones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es pensar críticamente]] ⭐
- [[Argumentos, premisas y conclusiones]]
- [[Falacias lógicas comunes]] ⭐
- [[Evaluar fuentes e información en internet]] ⭐

## Progreso

```base
filters:
  and:
    - file.inFolder("47-Pensamiento-Critico/01 Pensamiento crítico")
    - file.name != "_Índice Pensamiento crítico"
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
