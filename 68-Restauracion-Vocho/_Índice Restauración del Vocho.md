---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Restauración del Vocho
tags: [indice, vocho]
---
# Restauración del Vocho

Avance y prioridades en [[Temario v2 - senior y arquitecto]].

> [!note]
> Meta: restaurar tu VW Sedán. Datos técnicos (según el año de tu vocho), trámites y verificación se confirman en manuales y fuentes oficiales de tu estado al escribir cada nota. La seguridad va primero: nunca trabajes bajo el auto sostenido solo con el gato.

## Secciones

- [[68-Restauracion-Vocho/01 Conocer tu vocho/_Índice Conocer tu vocho|Conocer tu vocho]]
- [[68-Restauracion-Vocho/02 Taller y seguridad/_Índice Taller y seguridad|Taller y seguridad]]
- [[68-Restauracion-Vocho/03 Motor/_Índice Motor|Motor]]
- [[68-Restauracion-Vocho/04 Transmisión y frenos/_Índice Transmisión y frenos|Transmisión y frenos]]
- [[68-Restauracion-Vocho/05 Suspensión y dirección/_Índice Suspensión y dirección|Suspensión y dirección]]
- [[68-Restauracion-Vocho/06 Sistema eléctrico/_Índice Sistema eléctrico|Sistema eléctrico]]
- [[68-Restauracion-Vocho/07 Carrocería y pintura/_Índice Carrocería y pintura|Carrocería y pintura]]
- [[68-Restauracion-Vocho/08 Interiores/_Índice Interiores|Interiores]]
- [[68-Restauracion-Vocho/09 Piezas y costos/_Índice Piezas y costos|Piezas y costos]]
- [[68-Restauracion-Vocho/10 Mejoras/_Índice Mejoras|Mejoras]]
- [[68-Restauracion-Vocho/11 Trámites/_Índice Trámites|Trámites]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("68-Restauracion-Vocho")
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
