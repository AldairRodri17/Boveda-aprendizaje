---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
contenido: completo
---
# Redes

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Redes.canvas|Mapa mental]].

## Secciones

- [[19-Redes/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[19-Redes/02 Switching/_Índice Switching|Switching]]
- [[19-Redes/03 Direccionamiento IP/_Índice Direccionamiento IP|Direccionamiento IP]]
- [[19-Redes/04 Servicios de red/_Índice Servicios de red|Servicios de red]]
- [[19-Redes/05 Routing/_Índice Routing|Routing]]
- [[19-Redes/06 Inalámbrico/_Índice Inalámbrico|Inalámbrico]]
- [[19-Redes/07 Seguridad de redes/_Índice Seguridad de redes|Seguridad de redes]]
- [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]
- [[19-Redes/09 Avanzado/_Índice Avanzado|Avanzado]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("19-Redes")
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
