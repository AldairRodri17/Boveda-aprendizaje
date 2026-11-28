---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fundamentos
tags: [indice, fundamentos]
contenido: completo
---
# Datos y formatos

Parte de: [[02-Fundamentos/_Índice Fundamentos|Fundamentos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[JSON]]
- [[XML]]
- [[CSV]]
- [[Base64]]
- [[UTF-8 y codificaciones]]
- [[Fechas, zonas horarias y UTC]]
- [[ISO 8601]]
- [[GUID y UUID]]
- [[Dinero y decimales]]
- [[Expresiones regulares]]
- [[Internacionalización y localización (es-MX)]]

## Laboratorios

- [[Lab 1 - Expresiones regulares y fechas en Python]]

## Progreso

```base
filters:
  and:
    - file.inFolder("02-Fundamentos/06 Datos y formatos")
    - file.name != "_Índice Datos y formatos"
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
