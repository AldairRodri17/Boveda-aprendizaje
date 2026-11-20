---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Mitología e historia mesoamericana y griega
tags: [indice, mitologia]
---
# Mitología e historia mesoamericana y griega

Avance y prioridades en [[Temario v2 - senior y arquitecto]].

> [!note]
> Incluye la historia detrás de los nombres de tus proyectos (marcados con 🛠).

## Secciones

- [[67-Mitologia/01 Mesoamérica/_Índice Mesoamérica|Mesoamérica]]
- [[67-Mitologia/02 Mexicas/_Índice Mexicas|Mexicas]]
- [[67-Mitologia/03 Mayas/_Índice Mayas|Mayas]]
- [[67-Mitologia/04 Otras culturas mesoamericanas/_Índice Otras culturas mesoamericanas|Otras culturas mesoamericanas]]
- [[67-Mitologia/05 Grecia/_Índice Grecia|Grecia]]
- [[67-Mitologia/06 Comparaciones/_Índice Comparaciones|Comparaciones]]
- [[67-Mitologia/07 Visitar y aprender más/_Índice Visitar y aprender más|Visitar y aprender más]]
- [[67-Mitologia/08 Para tus proyectos/_Índice Para tus proyectos|Para tus proyectos]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("67-Mitologia")
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
