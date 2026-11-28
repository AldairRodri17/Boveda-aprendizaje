---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: SQL Server
tags: [indice, sql-server]
contenido: completo
---
# Básico

Parte de: [[06-SQL-Server/_Índice SQL Server|SQL Server]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[SQL Server]]
- [[Modelo relacional]]
- [[Tipos de datos (varchar vs nvarchar, decimal vs float)]]
- [[datetime2 y fechas en SQL Server]]
- [[uniqueidentifier]]
- [[Esquemas (schemas) en SQL Server]]
- [[CREATE, ALTER y DROP (DDL)]]
- [[INSERT, UPDATE y DELETE (DML)]]
- [[SELECT]]
- [[WHERE, LIKE, IN y BETWEEN]]
- [[ORDER BY, TOP y OFFSET FETCH]]
- [[DISTINCT]]
- [[NULL y lógica de tres valores]]
- [[Funciones de texto]]
- [[Funciones de fecha]]
- [[CASE]]
- [[Funciones de agregación]]
- [[GROUP BY y HAVING]]
- [[JOINs]]
- [[INNER JOIN]]
- [[LEFT y RIGHT JOIN]]
- [[FULL JOIN y CROSS JOIN]]
- [[Self join]]
- [[UNION, UNION ALL, INTERSECT y EXCEPT]]
- [[Claves primarias y foráneas]]
- [[UNIQUE, CHECK y DEFAULT]]
- [[IDENTITY vs SEQUENCE]]
- [[Normalización]]
  - [[Primera forma normal (1FN)]]
  - [[Segunda forma normal (2FN)]]
  - [[Tercera forma normal (3FN)]]
  - [[Forma normal de Boyce-Codd (FNBC)]]
  - [[Desnormalización deliberada]]

## Laboratorios

- [[Lab 1 - Base de práctica Tienda en SQL Server]]
- [[Lab 2 - Consultas de agregación y JOINs en Tienda]]

## Progreso

```base
filters:
  and:
    - file.inFolder("06-SQL-Server/01 Básico")
    - file.name != "_Índice Básico"
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
