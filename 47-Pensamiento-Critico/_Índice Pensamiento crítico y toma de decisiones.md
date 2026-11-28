---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Pensamiento crítico y toma de decisiones
tags: [indice, pensamiento-critico]
---
# Pensamiento crítico y toma de decisiones

Avance y prioridades en [[Temario]].

## Secciones

- [[47-Pensamiento-Critico/01 Pensamiento crítico/_Índice Pensamiento crítico|Pensamiento crítico]]
- [[47-Pensamiento-Critico/02 Sesgos cognitivos/_Índice Sesgos cognitivos|Sesgos cognitivos]]
- [[47-Pensamiento-Critico/03 Modelos mentales/_Índice Modelos mentales|Modelos mentales]]
- [[47-Pensamiento-Critico/04 Decisiones/_Índice Decisiones|Decisiones]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("47-Pensamiento-Critico")
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
