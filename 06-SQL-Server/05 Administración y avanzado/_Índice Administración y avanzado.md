---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: SQL Server
tags: [indice, sql-server]
---
# Administración y avanzado

Parte de: [[06-SQL-Server/_Índice SQL Server|SQL Server]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Tablas temporales del sistema (temporal tables)]]
- [[Particionamiento]]
- [[Roles y permisos]]
- [[Cifrado y Always Encrypted]]
- [[Row-level security]]
- [[Backup y restore]]
  - [[Backup completo, diferencial y de log]]
  - [[Restauración a un punto en el tiempo]]
- [[Modelos de recuperación]]
  - [[Modelo de recuperación SIMPLE]]
  - [[Modelo de recuperación FULL]]
  - [[Modelo de recuperación BULK_LOGGED]]
- [[SQL Server Agent y jobs]]
- [[Linked servers]]
- [[Collation]]
- [[Auditoría (CDC y Change Tracking)]]
- [[Migraciones de base de datos (scripts versionados, DACPAC, DbUp y Flyway)]]
- [[Migraciones sin caídas (expand y contract)]]
- [[Multi-tenancy]] (en 09-Arquitectura-Patrones)
- [[Azure SQL - diferencias con SQL Server]]
- [[Azure SQL serverless (auto-pausa y reintentos)]]
- [[PostgreSQL]]
- [[Funciones y procedimientos en PostgreSQL]]
- [[Snapshot inmutable versionado]]

## Laboratorios

- [[Lab 9 - Script de migración idempotente con tabla de versiones]]
- [[Lab 10 - Usuario de aplicación con permisos mínimos por esquema]]

## Progreso

```base
filters:
  and:
    - file.inFolder("06-SQL-Server/05 Administración y avanzado")
    - file.name != "_Índice Administración y avanzado"
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
