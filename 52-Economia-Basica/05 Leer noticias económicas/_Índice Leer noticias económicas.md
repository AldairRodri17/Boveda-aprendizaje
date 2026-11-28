---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Economía básica
tags: [indice, economia]
contenido: completo
---
# Leer noticias económicas

Parte de: [[52-Economia-Basica/_Índice Economía básica|Economía básica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cómo leer una noticia económica]] ⭐
- [[Indicadores para seguir (inflación, tasa, empleo y tipo de cambio)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("52-Economia-Basica/05 Leer noticias económicas")
    - file.name != "_Índice Leer noticias económicas"
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
