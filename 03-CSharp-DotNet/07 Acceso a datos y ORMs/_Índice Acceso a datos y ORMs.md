---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: C# y .NET
tags: [indice, csharp-dotnet]
---
# Acceso a datos y ORMs

Parte de: [[03-CSharp-DotNet/_Índice CSharp y .NET|C# y .NET]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es un ORM y cuándo usarlo]]
- [[ADO.NET]]
- [[Dapper]]
- [[Dapper - QueryMultiple y multi-mapping]]
- [[Dapper - transacciones]]
- [[Dapper - table-valued parameters]]
- [[SQL en archivos con una interfaz por consulta]]
- [[Entity Framework Core]]
- [[EF Core - DbContext]]
- [[EF Core - migraciones]]
- [[EF Core - tracking vs no tracking]]
- [[EF Core - relaciones]]
- [[EF Core - carga eager, lazy y explícita]]
- [[Problema N+1]]
- [[EF Core - SQL crudo]]
- [[Dapper vs EF Core vs ADO.NET]]
- [[Repository]]
- [[Connection pooling]]
- [[Parametrización y SQL injection]]
- [[Patrón Outbox]]
- [[SQLite]]

## Laboratorios

- [[Lab 9 - Dapper con stored procedures y transacción]]

## Progreso

```base
filters:
  and:
    - file.inFolder("03-CSharp-DotNet/07 Acceso a datos y ORMs")
    - file.name != "_Índice Acceso a datos y ORMs"
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
