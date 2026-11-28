---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Restauración del Vocho
tags: [indice, vocho]
contenido: completo
---
# Trámites

Parte de: [[68-Restauracion-Vocho/_Índice Restauración del Vocho|Restauración del Vocho]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Verificación vehicular y placas]]
- [[Placas de auto antiguo (según el estado)]]
- [[Seguro para el vocho]]

## Progreso

```base
filters:
  and:
    - file.inFolder("68-Restauracion-Vocho/11 Trámites")
    - file.name != "_Índice Trámites"
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
