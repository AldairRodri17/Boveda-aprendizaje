---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: SQL Server
tags: [indice, sql-server]
contenido: completo
---
# Rendimiento

Parte de: [[06-SQL-Server/_Índice SQL Server|SQL Server]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Índices]]
- [[Índice clustered]]
- [[Índice nonclustered]]
- [[Columnas incluidas y filtered indexes]]
- [[Columnstore]]
- [[Planes de ejecución]]
  - [[Plan estimado vs plan real]]
  - [[Operadores de acceso (Scan, Seek y Key Lookup)]]
  - [[Operadores de join (Nested Loops, Hash Match y Merge Join)]]
  - [[Advertencias del plan (conversiones implícitas y spills)]]
- [[Estadísticas]]
- [[SARGability]]
- [[Parameter sniffing]]
- [[Query Store]]
- [[DMVs]]
- [[Extended Events]]

## Laboratorios

- [[Lab 7 - Índices y planes de ejecución antes y después]]
- [[Lab 8 - Parameter sniffing y Query Store]]

## Progreso

```base
filters:
  and:
    - file.inFolder("06-SQL-Server/04 Rendimiento")
    - file.name != "_Índice Rendimiento"
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
