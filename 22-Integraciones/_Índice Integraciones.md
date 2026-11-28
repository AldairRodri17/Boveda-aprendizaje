---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Integraciones
tags: [indice, integraciones]
contenido: completo
---
# Integraciones

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Integraciones.canvas|Mapa mental]].

## Secciones

- [[22-Integraciones/01 Pagos/_Índice Pagos|Pagos]]
- [[22-Integraciones/02 Correo/_Índice Correo|Correo]]
- [[22-Integraciones/03 Documentos y archivos/_Índice Documentos y archivos|Documentos y archivos]]
- [[22-Integraciones/04 Escritorio y hardware/_Índice Escritorio y hardware|Escritorio y hardware]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("22-Integraciones")
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
