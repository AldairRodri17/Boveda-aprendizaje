---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Automatización, DevOps y operación
tags: [indice, automatizacion-devops]
contenido: completo
---
# Automatización, DevOps y operación

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Automatización, DevOps y operación.canvas|Mapa mental]].

## Secciones

- [[08-Automatizacion-DevOps/01 Automatización y CICD/_Índice Automatización y CICD|Automatización y CICD]]
- [[08-Automatizacion-DevOps/02 Operación en producción/_Índice Operación en producción|Operación en producción]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("08-Automatizacion-DevOps")
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
