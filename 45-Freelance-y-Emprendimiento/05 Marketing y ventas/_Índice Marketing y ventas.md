---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Freelance y emprendimiento
tags: [indice, freelance]
contenido: completo
---
# Marketing y ventas

Parte de: [[45-Freelance-y-Emprendimiento/_Índice Freelance y emprendimiento|Freelance y emprendimiento]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Marketing básico para independientes]]
- [[Portafolio que vende]]
- [[Redes sociales para un negocio pequeño]]
- [[Atención al cliente y reseñas]]

## Progreso

```base
filters:
  and:
    - file.inFolder("45-Freelance-y-Emprendimiento/05 Marketing y ventas")
    - file.name != "_Índice Marketing y ventas"
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
