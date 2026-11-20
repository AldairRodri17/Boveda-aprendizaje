---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Inversiones
tags: [indice, inversiones]
---
# Inversiones

Avance y prioridades en [[Temario v2 - senior y arquitecto]].

> [!note]
> Contenido educativo, no asesoría de inversión. Tasas, comisiones y reglas fiscales se verifican en la fuente oficial al escribir cada nota.

## Secciones

- [[42-Inversiones/01 Conceptos/_Índice Conceptos|Conceptos]]
- [[42-Inversiones/02 Instrumentos de deuda/_Índice Instrumentos de deuda|Instrumentos de deuda]]
- [[42-Inversiones/03 Renta variable/_Índice Renta variable|Renta variable]]
- [[42-Inversiones/04 Otros activos/_Índice Otros activos|Otros activos]]
- [[42-Inversiones/05 Estrategia/_Índice Estrategia|Estrategia]]
- [[42-Inversiones/06 Práctica/_Índice Práctica|Práctica]]
- [[42-Inversiones/07 Psicología y errores/_Índice Psicología y errores|Psicología y errores]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("42-Inversiones")
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
