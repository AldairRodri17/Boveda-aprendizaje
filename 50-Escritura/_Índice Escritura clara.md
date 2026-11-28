---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Escritura clara
tags: [indice, escritura]
---
# Escritura clara

Avance y prioridades en [[Temario]].

> [!note]
> Empieza por ortografía y caligrafía. La caligrafía se califica con fotos de tus planas y la [[Rúbrica de caligrafía]].

## Secciones

- [[50-Escritura/01 Ortografía/_Índice Ortografía|Ortografía]]
- [[50-Escritura/02 Puntuación/_Índice Puntuación|Puntuación]]
- [[50-Escritura/03 Gramática para escribir bien/_Índice Gramática para escribir bien|Gramática para escribir bien]]
- [[50-Escritura/04 Caligrafía/_Índice Caligrafía|Caligrafía]]
- [[50-Escritura/05 Redacción clara/_Índice Redacción clara|Redacción clara]]
- [[50-Escritura/06 Escritura profesional/_Índice Escritura profesional|Escritura profesional]]
- [[50-Escritura/07 Escritura creativa (opcional)/_Índice Escritura creativa (opcional)|Escritura creativa (opcional)]]
- [[50-Escritura/08 Herramientas/_Índice Herramientas|Herramientas]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("50-Escritura")
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
