---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Freelance y emprendimiento
tags: [indice, freelance]
contenido: completo
---
# Freelance y emprendimiento

Avance y prioridades en [[Temario]].

> [!note]
> Reglas fiscales y permisos se verifican en la fuente oficial (SAT, AFAC) al escribir cada nota.

## Secciones

- [[45-Freelance-y-Emprendimiento/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[45-Freelance-y-Emprendimiento/02 Software como freelance/_Índice Software como freelance|Software como freelance]]
- [[45-Freelance-y-Emprendimiento/03 Monetizar hobbies/_Índice Monetizar hobbies|Monetizar hobbies]]
- [[45-Freelance-y-Emprendimiento/04 Legal y fiscal/_Índice Legal y fiscal|Legal y fiscal]]
- [[45-Freelance-y-Emprendimiento/05 Marketing y ventas/_Índice Marketing y ventas|Marketing y ventas]]
- [[45-Freelance-y-Emprendimiento/06 Finanzas del negocio/_Índice Finanzas del negocio|Finanzas del negocio]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("45-Freelance-y-Emprendimiento")
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
