---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Seguridad
tags: [indice, seguridad]
---
# Seguridad

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Seguridad.canvas|Mapa mental]].

## Secciones

- [[12-Seguridad/01 Fundamentos de seguridad/_Índice Fundamentos de seguridad|Fundamentos de seguridad]]
- [[12-Seguridad/02 Inyección SQL/_Índice Inyección SQL|Inyección SQL]]
- [[12-Seguridad/03 XSS/_Índice XSS|XSS]]
- [[12-Seguridad/04 Otras vulnerabilidades web/_Índice Otras vulnerabilidades web|Otras vulnerabilidades web]]
- [[12-Seguridad/05 Herramientas defensivas/_Índice Herramientas defensivas|Herramientas defensivas]]
- [[12-Seguridad/06 Hacking ético/_Índice Hacking ético|Hacking ético]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("12-Seguridad")
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
