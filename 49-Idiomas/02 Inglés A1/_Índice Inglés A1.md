---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
contenido: completo
---
# Inglés A1

Parte de: [[49-Idiomas/_Índice Idiomas|Idiomas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Inglés A1 - pronombres y verbo to be]]
- [[Inglés A1 - presente simple]] ⭐
- [[Inglés A1 - artículos y sustantivos]]
- [[Inglés A1 - there is y there are]]
- [[Inglés A1 - preguntas con wh]]
- [[Inglés A1 - vocabulario básico (números, fechas, familia y rutina)]]
- [[Inglés A1 - presentarte y hablar de tu rutina]] ⭐

## Progreso

```base
filters:
  and:
    - file.inFolder("49-Idiomas/02 Inglés A1")
    - file.name != "_Índice Inglés A1"
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
