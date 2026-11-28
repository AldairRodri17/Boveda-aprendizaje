---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Viajes y fotografía de viaje
tags: [indice, viajes]
---
# Viajes y fotografía de viaje

Avance y prioridades en [[Temario]].

> [!note]
> Requisitos de pasaporte, visas y reglas para volar drones o grabar en zonas protegidas se verifican en la fuente oficial al escribir cada nota.

## Secciones

- [[65-Viajes/01 Planeación/_Índice Planeación|Planeación]]
- [[65-Viajes/02 Documentos y salud/_Índice Documentos y salud|Documentos y salud]]
- [[65-Viajes/03 Dinero en viaje/_Índice Dinero en viaje|Dinero en viaje]]
- [[65-Viajes/04 Viajar con equipo/_Índice Viajar con equipo|Viajar con equipo]]
- [[65-Viajes/05 Fotografía de viaje/_Índice Fotografía de viaje|Fotografía de viaje]]
- [[65-Viajes/06 Destinos fotográficos/_Índice Destinos fotográficos|Destinos fotográficos]]
- [[65-Viajes/07 Seguridad/_Índice Seguridad|Seguridad]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("65-Viajes")
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
