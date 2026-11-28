---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Filosofía práctica
tags: [indice, filosofia]
---
# Filosofía práctica

Avance y prioridades en [[Temario]].

## Secciones

- [[66-Filosofia-Practica/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[66-Filosofia-Practica/02 Estoicismo/_Índice Estoicismo|Estoicismo]]
- [[66-Filosofia-Practica/03 Otras escuelas/_Índice Otras escuelas|Otras escuelas]]
- [[66-Filosofia-Practica/04 Ética/_Índice Ética|Ética]]
- [[66-Filosofia-Practica/05 Vivir con propósito/_Índice Vivir con propósito|Vivir con propósito]]
- [[66-Filosofia-Practica/06 Práctica/_Índice Práctica|Práctica]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("66-Filosofia-Practica")
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
