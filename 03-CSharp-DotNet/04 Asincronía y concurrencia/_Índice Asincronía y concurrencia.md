---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: C# y .NET
tags: [indice, csharp-dotnet]
---
# Asincronía y concurrencia

Parte de: [[03-CSharp-DotNet/_Índice CSharp y .NET|C# y .NET]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Task y async-await]]
- [[Cómo funciona async por dentro (máquina de estados)]]
- [[ValueTask]]
- [[CancellationToken]]
- [[ConfigureAwait]]
- [[async void y por qué evitarlo]]
- [[Sync over async y thread pool starvation]]
- [[Task.WhenAll y Task.WhenAny]]
- [[Hilos y ThreadPool]]
- [[lock y concurrencia]]
- [[Parallel y PLINQ]]
- [[System.Threading.Channels]]
- [[Span y Memory]]

## Laboratorios

- [[Lab 5 - async-await, CancellationToken y sync over async]]

## Progreso

```base
filters:
  and:
    - file.inFolder("03-CSharp-DotNet/04 Asincronía y concurrencia")
    - file.name != "_Índice Asincronía y concurrencia"
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
