---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Ciberseguridad personal y vida digital
tags: [indice, ciberseguridad-personal]
---
# Ciberseguridad personal y vida digital

Avance y prioridades en [[Temario]].

> [!note]
> La teoría a fondo está en 12 · Seguridad; aquí va lo práctico para tu vida diaria.

## Secciones

- [[51-Ciberseguridad-Personal/01 Contraseñas y cuentas/_Índice Contraseñas y cuentas|Contraseñas y cuentas]]
- [[51-Ciberseguridad-Personal/02 Fraudes y engaños/_Índice Fraudes y engaños|Fraudes y engaños]]
- [[51-Ciberseguridad-Personal/03 Dispositivos/_Índice Dispositivos|Dispositivos]]
- [[51-Ciberseguridad-Personal/04 Respaldo y vida digital/_Índice Respaldo y vida digital|Respaldo y vida digital]]
- [[51-Ciberseguridad-Personal/05 Privacidad/_Índice Privacidad|Privacidad]]
- [[51-Ciberseguridad-Personal/06 Si te hackean/_Índice Si te hackean|Si te hackean]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("51-Ciberseguridad-Personal")
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
