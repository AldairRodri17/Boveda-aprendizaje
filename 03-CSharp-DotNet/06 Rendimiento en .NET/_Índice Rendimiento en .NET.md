---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: C# y .NET
tags: [indice, csharp-dotnet]
---
# Rendimiento en .NET

Parte de: [[03-CSharp-DotNet/_Índice CSharp y .NET|C# y .NET]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[BenchmarkDotNet]]
- [[dotnet-counters, dotnet-trace y dotnet-dump]]
  - [[dotnet-counters]]
  - [[dotnet-trace]]
  - [[dotnet-dump]]
- [[Allocations y modos del GC]]
- [[JIT y tiered compilation]]

## Laboratorios

- [[Lab 8 - BenchmarkDotNet midiendo allocations]]

## Progreso

```base
filters:
  and:
    - file.inFolder("03-CSharp-DotNet/06 Rendimiento en .NET")
    - file.name != "_Índice Rendimiento en .NET"
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
