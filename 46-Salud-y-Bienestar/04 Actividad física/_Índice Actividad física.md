---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud y bienestar
tags: [indice, salud]
---
# Actividad física

Parte de: [[46-Salud-y-Bienestar/_Índice Salud y bienestar|Salud y bienestar]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Ejercicio básico para quien trabaja sentado]] ⭐
- [[Moverse durante el día]]
- [[Fuerza y movilidad en casa]]

## Progreso

```base
filters:
  and:
    - file.inFolder("46-Salud-y-Bienestar/04 Actividad física")
    - file.name != "_Índice Actividad física"
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
