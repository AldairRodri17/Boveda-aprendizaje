---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: SQL Server
tags: [indice, sql-server]
contenido: completo
---
# Intermedio

Parte de: [[06-SQL-Server/_Índice SQL Server|SQL Server]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Subconsultas]]
- [[Subconsultas correlacionadas]]
- [[EXISTS vs IN]]
- [[CTE]]
- [[CTE múltiples]]
- [[CTE recursivas]]
- [[CROSS APPLY y OUTER APPLY]]
- [[Funciones de ventana]]
- [[ROW_NUMBER, RANK y DENSE_RANK]]
- [[LAG y LEAD]]
- [[Totales acumulados con OVER]]
- [[PIVOT y UNPIVOT]]
- [[STRING_AGG y STRING_SPLIT]]
- [[JSON en SQL Server (OPENJSON y FOR JSON)]]
- [[Nivel de compatibilidad de la base]]
- [[XML en SQL Server]]
- [[MERGE]]
- [[OUTPUT]]
- [[Tablas temporales y variables de tabla]]
- [[Table-valued parameters]]
- [[Vistas]]
- [[Funciones escalares y de tabla]]
- [[Stored procedures]]
- [[CREATE OR ALTER y scripts idempotentes]]
- [[Lotes con GO, sqlcmd e Invoke-Sqlcmd]]
- [[Parámetros de salida y RETURN]]
- [[SET NOCOUNT y SET XACT_ABORT]]
- [[TRY CATCH y THROW]]
- [[Triggers]]
- [[Cursores y por qué evitarlos]]
- [[SQL dinámico y sp_executesql]]
- [[Paginación eficiente]]
- [[Borrado lógico y auditoría]]

## Laboratorios

- [[Lab 3 - CTE y funciones de ventana en Tienda]]
- [[Lab 4 - Stored procedure transaccional con TRY CATCH y XACT_ABORT]]

## Progreso

```base
filters:
  and:
    - file.inFolder("06-SQL-Server/02 Intermedio")
    - file.name != "_Índice Intermedio"
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
