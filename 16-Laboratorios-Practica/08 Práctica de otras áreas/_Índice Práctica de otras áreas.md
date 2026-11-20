---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Laboratorios y práctica
tags: [indice, laboratorios-practica]
---
# Práctica de otras áreas

Parte de: [[16-Laboratorios-Practica/_Índice Laboratorios y práctica|Laboratorios y práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Microsoft Learn - rutas gratuitas de Azure]]
- [[Retos de SQL (plataformas de práctica)]]
- [[Retos de JavaScript y CSharp (plataformas de práctica)]]
- [[Retos de diseño de sistemas]]
- [[Proyectos propios como laboratorio de aprendizaje]]

## Progreso

```base
filters:
  and:
    - file.inFolder("16-Laboratorios-Practica/08 Práctica de otras áreas")
    - file.name != "_Índice Práctica de otras áreas"
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
