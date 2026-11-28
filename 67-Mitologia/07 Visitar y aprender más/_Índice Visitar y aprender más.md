---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Mitología e historia mesoamericana y griega
tags: [indice, mitologia]
---
# Visitar y aprender más

Parte de: [[67-Mitologia/_Índice Mitología e historia mesoamericana y griega|Mitología e historia mesoamericana y griega]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Museo Nacional de Antropología]] ⭐
- [[Zonas arqueológicas para visitar]] ⭐
- [[Libros y documentales de mitología e historia]]

## Laboratorios

- [[Lab 2 - Visita al Museo de Antropología con fotos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("67-Mitologia/07 Visitar y aprender más")
    - file.name != "_Índice Visitar y aprender más"
views:
  - type: table
    name: Temas
    filters:
      and:
        - tipo != "laboratorio"
    order:
      - file.name
      - progreso
      - nivel
  - type: table
    name: Laboratorios
    filters:
      and:
        - tipo == "laboratorio"
    order:
      - file.name
      - progreso
```
