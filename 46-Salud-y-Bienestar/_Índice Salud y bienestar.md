---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud y bienestar
tags: [indice, salud]
---
# Salud y bienestar

Avance y prioridades en [[Temario]].

> [!note]
> Información general para quien trabaja sentado. Lo médico se consulta con un profesional de la salud.

## Secciones

- [[46-Salud-y-Bienestar/01 Ergonomía/_Índice Ergonomía|Ergonomía]]
- [[46-Salud-y-Bienestar/02 Salud visual/_Índice Salud visual|Salud visual]]
- [[46-Salud-y-Bienestar/03 Sueño/_Índice Sueño|Sueño]]
- [[46-Salud-y-Bienestar/04 Actividad física/_Índice Actividad física|Actividad física]]
- [[46-Salud-y-Bienestar/05 Alimentación/_Índice Alimentación|Alimentación]]
- [[46-Salud-y-Bienestar/06 Salud mental/_Índice Salud mental|Salud mental]]
- [[46-Salud-y-Bienestar/07 Prevención/_Índice Prevención|Prevención]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("46-Salud-y-Bienestar")
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
