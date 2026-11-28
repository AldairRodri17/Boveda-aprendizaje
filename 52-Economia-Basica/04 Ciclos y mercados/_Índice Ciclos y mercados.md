---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Economía básica
tags: [indice, economia]
---
# Ciclos y mercados

Parte de: [[52-Economia-Basica/_Índice Economía básica|Economía básica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Ciclos económicos y recesiones]] ⭐
- [[Mercados financieros y economía real]]
- [[Deuda pública y calificación crediticia]]

## Progreso

```base
filters:
  and:
    - file.inFolder("52-Economia-Basica/04 Ciclos y mercados")
    - file.name != "_Índice Ciclos y mercados"
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
