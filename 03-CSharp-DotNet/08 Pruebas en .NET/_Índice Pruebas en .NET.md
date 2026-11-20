---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: C# y .NET
tags: [indice, csharp-dotnet]
---
# Pruebas en .NET

Parte de: [[03-CSharp-DotNet/_Índice CSharp y .NET|C# y .NET]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[xUnit]]
- [[Mocks, stubs y fakes]]
- [[Moq y NSubstitute]]
- [[Pruebas de integración con WebApplicationFactory]]
- [[Testcontainers]]
- [[TimeProvider para probar fechas]]
- [[Pruebas de reglas de arquitectura (NetArchTest)]]

## Laboratorios

- [[Lab 10 - Pruebas con xUnit y WebApplicationFactory]]

## Progreso

```base
filters:
  and:
    - file.inFolder("03-CSharp-DotNet/08 Pruebas en .NET")
    - file.name != "_Índice Pruebas en .NET"
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
