---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Desarrollo de videojuegos
tags: [indice, videojuegos]
---
# Unity

Parte de: [[60-Videojuegos/_Índice Desarrollo de videojuegos|Desarrollo de videojuegos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Unity - interfaz, escenas y GameObjects]]
- [[C# en Unity (MonoBehaviour y ciclo de vida)]] ⭐
- [[Física en Unity]]
- [[Prefabs en Unity]]
- [[Licencia de Unity]]

## Progreso

```base
filters:
  and:
    - file.inFolder("60-Videojuegos/03 Unity")
    - file.name != "_Índice Unity"
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
