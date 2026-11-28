---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: C# y .NET
tags: [indice, csharp-dotnet]
contenido: completo
---
# Orientación a objetos

Parte de: [[03-CSharp-DotNet/_Índice CSharp y .NET|C# y .NET]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Clases y objetos]]
- [[Propiedades, campos y modificadores de acceso]]
- [[Constructores]]
- [[Encapsulamiento]]
- [[Herencia]]
- [[Polimorfismo]]
- [[Clases abstractas]]
- [[Interfaces]]
- [[Interfaz vs clase abstracta]]
- [[Programar contra interfaces]]
- [[Implementación explícita de interfaces]]
- [[Implementaciones por defecto en interfaces]]
- [[Clases estáticas y miembros estáticos]]
- [[sealed y override]]
- [[Clases parciales]]
- [[struct]]
- [[record]]
- [[record struct]]
- [[init y required]]
- [[Igualdad por valor vs por referencia]]
- [[Tuplas y deconstrucción]]
- [[Clases anidadas y DTOs]]

## Laboratorios

- [[Lab 2 - Modelo de dominio con encapsulamiento y records]]

## Progreso

```base
filters:
  and:
    - file.inFolder("03-CSharp-DotNet/02 Orientación a objetos")
    - file.name != "_Índice Orientación a objetos"
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
