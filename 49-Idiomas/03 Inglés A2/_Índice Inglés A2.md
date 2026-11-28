---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
contenido: completo
---
# Inglés A2

Parte de: [[49-Idiomas/_Índice Idiomas|Idiomas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Inglés A2 - pasado simple (regulares e irregulares)]] ⭐
- [[Inglés A2 - presente continuo]]
- [[Inglés A2 - futuro con will y going to]]
- [[Inglés A2 - comparativos y superlativos]]
- [[Inglés A2 - preposiciones de tiempo y lugar]]
- [[Inglés A2 - can, could y should]]
- [[Inglés A2 - vocabulario de trabajo, viajes y compras]]

## Progreso

```base
filters:
  and:
    - file.inFolder("49-Idiomas/03 Inglés A2")
    - file.name != "_Índice Inglés A2"
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
