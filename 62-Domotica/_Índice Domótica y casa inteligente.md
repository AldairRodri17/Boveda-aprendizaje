---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Domótica y casa inteligente
tags: [indice, domotica]
---
# Domótica y casa inteligente

Avance y prioridades en [[Temario v2 - senior y arquitecto]].

> [!note]
> Junta homelab (61), electrónica (63) e impresión 3D (36).

## Secciones

- [[62-Domotica/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[62-Domotica/02 Home Assistant/_Índice Home Assistant|Home Assistant]]
- [[62-Domotica/03 Dispositivos/_Índice Dispositivos|Dispositivos]]
- [[62-Domotica/04 Hazlo tú mismo/_Índice Hazlo tú mismo|Hazlo tú mismo]]
- [[62-Domotica/05 Proyectos/_Índice Proyectos|Proyectos]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("62-Domotica")
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
