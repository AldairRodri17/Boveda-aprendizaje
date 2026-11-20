---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Hablar en público y exposiciones
tags: [indice, hablar-en-publico]
---
# Hablar en público y exposiciones

Avance y prioridades en [[Temario v2 - senior y arquitecto]].

> [!note]
> La calificación usa la [[Rúbrica de exposiciones]]: teoría → visto; sesiones de práctica → practicado; presentación real o tres sesiones con 3.5+ → dominado.

## Secciones

- [[40-Hablar-en-Publico/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[40-Hablar-en-Publico/02 Estructura del mensaje/_Índice Estructura del mensaje|Estructura del mensaje]]
- [[40-Hablar-en-Publico/03 Voz y lenguaje corporal/_Índice Voz y lenguaje corporal|Voz y lenguaje corporal]]
- [[40-Hablar-en-Publico/04 Apoyos visuales/_Índice Apoyos visuales|Apoyos visuales]]
- [[40-Hablar-en-Publico/05 Formatos/_Índice Formatos|Formatos]]
- [[40-Hablar-en-Publico/06 Presentar en línea/_Índice Presentar en línea|Presentar en línea]]
- [[40-Hablar-en-Publico/07 Preguntas y respuestas/_Índice Preguntas y respuestas|Preguntas y respuestas]]
- [[40-Hablar-en-Publico/08 Práctica y evaluación/_Índice Práctica y evaluación|Práctica y evaluación]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("40-Hablar-en-Publico")
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
