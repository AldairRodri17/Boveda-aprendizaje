---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
---
# Inglés B2

Parte de: [[49-Idiomas/_Índice Idiomas|Idiomas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Inglés B2 - presente perfecto continuo y pasado perfecto]]
- [[Inglés B2 - tercer condicional y condicionales mixtos]]
- [[Inglés B2 - estilo indirecto (reported speech)]]
- [[Inglés B2 - cláusulas relativas]]
- [[Inglés B2 - conectores para hablar y escribir]] ⭐
- [[Inglés B2 - collocations]]
- [[Inglés B2 - debatir y argumentar]] ⭐

## Progreso

```base
filters:
  and:
    - file.inFolder("49-Idiomas/05 Inglés B2")
    - file.name != "_Índice Inglés B2"
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
