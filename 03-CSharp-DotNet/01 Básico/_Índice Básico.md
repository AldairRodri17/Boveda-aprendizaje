---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: C# y .NET
tags: [indice, csharp-dotnet]
contenido: completo
---
# Básico

Parte de: [[03-CSharp-DotNet/_Índice CSharp y .NET|C# y .NET]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[NET y CSharp]]
- [[Qué es .NET, el CLR y el SDK]]
- [[Solución, proyecto y estructura de carpetas]]
- [[Tipos de valor y de referencia]]
- [[Tipos primitivos]]
- [[string e inmutabilidad]]
- [[var y tipado implícito]]
- [[null y nullable reference types]]
- [[Nullable de valor (Nullable de int)]]
- [[Operadores de null en CSharp]]
- [[Condicionales y bucles]]
- [[Métodos y parámetros (ref, out, in, params)]]
- [[Parámetros opcionales y nombrados]]
- [[Arrays]]
- [[Colecciones (List, Dictionary, HashSet, Queue, Stack)]]
  - [[List de T]]
  - [[Dictionary de TKey y TValue]]
  - [[HashSet de T]]
  - [[Queue y Stack de T]]
- [[Enums]]
- [[Excepciones (try, catch, finally, throw)]]
- [[using y namespaces]]
- [[Conversión de tipos (casting, parse, TryParse)]]
- [[DateTime vs DateTimeOffset]]

## Laboratorios

- [[Lab 1 - Consola de inventario con colecciones y TryParse]]

## Progreso

```base
filters:
  and:
    - file.inFolder("03-CSharp-DotNet/01 Básico")
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
