---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: SQL Server
tags: [indice, sql-server]
---
# Transacciones y concurrencia

Parte de: [[06-SQL-Server/_Índice SQL Server|SQL Server]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Propiedades ACID]]
- [[Transacciones (BEGIN, COMMIT y ROLLBACK)]]
- [[Transacciones anidadas y savepoints]]
- [[Niveles de aislamiento]]
  - [[READ UNCOMMITTED y NOLOCK]]
  - [[READ COMMITTED (nivel por defecto)]]
  - [[REPEATABLE READ]]
  - [[SERIALIZABLE y bloqueos de rango]]
- [[Bloqueos (locks)]]
- [[Deadlocks]]
- [[Snapshot y RCSI]]
- [[Concurrencia optimista]]

## Laboratorios

- [[Lab 5 - Concurrencia optimista con rowversion en dos sesiones]]
- [[Lab 6 - Bloqueos, niveles de aislamiento y deadlock en dos sesiones]]

## Progreso

```base
filters:
  and:
    - file.inFolder("06-SQL-Server/03 Transacciones y concurrencia")
    - file.name != "_Índice Transacciones y concurrencia"
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
