---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: C# y .NET
tags: [indice, csharp-dotnet]
---
# Intermedio

Parte de: [[03-CSharp-DotNet/_Índice CSharp y .NET|C# y .NET]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Genéricos]]
- [[Constraints de genéricos]]
- [[Varianza (in y out) en genéricos]]
- [[Delegates]]
- [[Events]]
- [[Func, Action y Predicate]]
- [[Expresiones lambda]]
- [[Métodos de extensión]]
- [[Pattern matching]]
  - [[Patrones de tipo y de propiedad en CSharp]]
  - [[Patrones relacionales y lógicos en CSharp]]
  - [[Patrones de lista en CSharp]]
- [[switch expressions]]
- [[IEnumerable, ICollection e IList]]
- [[IEnumerable vs IQueryable]]
- [[LINQ]]
  - [[LINQ - Where, Select y SelectMany]]
  - [[LINQ - GroupBy y Join]]
  - [[LINQ - agregaciones (Count, Sum, Aggregate)]]
  - [[LINQ - First, Single, Any y All]]
- [[Ejecución diferida en LINQ]]
- [[yield]]
- [[IAsyncEnumerable]]
- [[Indexadores]]
- [[Sobrecarga de operadores]]
- [[Atributos]]
- [[Reflexión]]
- [[Recursos embebidos (EmbeddedResource)]]
- [[Source generators y GeneratedRegex]]
- [[nameof y otras utilidades del lenguaje]]
- [[Novedades recientes de CSharp (collection expressions, primary constructors y más)]]
- [[StringBuilder]]
- [[IDisposable y using]]
- [[Recolector de basura]]
- [[Manejo de archivos y streams]]
- [[Serialización con System.Text.Json]]

## Laboratorios

- [[Lab 3 - Consola con LINQ y genéricos]]
- [[Lab 4 - Registro automático de sentencias SQL por reflexión]]

## Progreso

```base
filters:
  and:
    - file.inFolder("03-CSharp-DotNet/03 Intermedio")
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
