---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: SQL Server
tags: [indice, sql-server]
contenido: completo
---
# Otros datos

Parte de: [[06-SQL-Server/_Índice SQL Server|SQL Server]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Redis más allá de la caché (locks, colas y contadores)]]
- [[Búsqueda de texto completo]]
- [[Modelado en NoSQL]]
- [[ETL (conceptos)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("06-SQL-Server/06 Otros datos")
    - file.name != "_Índice Otros datos"
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
