---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Legal básico personal
tags: [indice, legal]
contenido: completo
---
# Legal básico personal

Avance y prioridades en [[Temario]].

> [!note]
> Contenido educativo, no asesoría legal. Leyes, montos y plazos se verifican en la fuente oficial (DOF, STPS, PROFECO, INDAUTOR, IMPI) al escribir cada nota.

## Secciones

- [[48-Legal-Personal/01 Derechos laborales/_Índice Derechos laborales|Derechos laborales]]
- [[48-Legal-Personal/02 Contratos/_Índice Contratos|Contratos]]
- [[48-Legal-Personal/03 Propiedad intelectual/_Índice Propiedad intelectual|Propiedad intelectual]]
- [[48-Legal-Personal/04 Consumidor y patrimonio/_Índice Consumidor y patrimonio|Consumidor y patrimonio]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("48-Legal-Personal")
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
