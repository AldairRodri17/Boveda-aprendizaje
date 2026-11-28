---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Idiomas
tags: [indice, idiomas]
---
# Pronunciación

Parte de: [[49-Idiomas/_Índice Idiomas|Idiomas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Sonidos del inglés que no existen en español]] ⭐
- [[Vocales cortas y largas en inglés]]
- [[Terminaciones -ed y -s]]
- [[Acento y ritmo de la oración en inglés]]
- [[Linking (unir palabras al hablar)]]

## Laboratorios

- [[Lab 3 - Shadowing de un video de 2 minutos]]

## Progreso

```base
filters:
  and:
    - file.inFolder("49-Idiomas/07 Pronunciación")
    - file.name != "_Índice Pronunciación"
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
