---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Escritura clara
tags: [indice, escritura]
---
# Escritura creativa (opcional)

Parte de: [[50-Escritura/_Índice Escritura clara|Escritura clara]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Contar una historia por escrito]]
- [[Guiones cortos para redes y video]]

## Progreso

```base
filters:
  and:
    - file.inFolder("50-Escritura/07 Escritura creativa (opcional)")
    - file.name != "_Índice Escritura creativa (opcional)"
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
