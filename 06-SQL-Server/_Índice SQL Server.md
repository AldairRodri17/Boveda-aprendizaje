---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: SQL Server
tags: [indice, sql-server]
contenido: completo
---
# SQL Server

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - SQL Server.canvas|Mapa mental]].

## Secciones

- [[06-SQL-Server/01 Básico/_Índice Básico|Básico]]
- [[06-SQL-Server/02 Intermedio/_Índice Intermedio|Intermedio]]
- [[06-SQL-Server/03 Transacciones y concurrencia/_Índice Transacciones y concurrencia|Transacciones y concurrencia]]
- [[06-SQL-Server/04 Rendimiento/_Índice Rendimiento|Rendimiento]]
- [[06-SQL-Server/05 Administración y avanzado/_Índice Administración y avanzado|Administración y avanzado]]
- [[06-SQL-Server/06 Otros datos/_Índice Otros datos|Otros datos]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("06-SQL-Server")
    - tipo != "indice"
views:
  - type: table
    name: Pendientes
    filters:
      and:
        - progreso != "visto"
        - progreso != "dominado"
    order:
      - file.name
      - file.folder
      - progreso
      - nivel
  - type: table
    name: Vistos
    filters:
      and:
        - progreso == "visto"
    order:
      - file.name
      - file.folder
  - type: table
    name: Dominados
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```
