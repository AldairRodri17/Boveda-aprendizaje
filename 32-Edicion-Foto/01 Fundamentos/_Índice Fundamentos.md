---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de fotografía
tags: [indice, edicion-foto]
contenido: completo
---
# Fundamentos

Parte de: [[32-Edicion-Foto/_Índice Edición de fotografía|Edición de fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Revelado RAW vs retoque]]
- [[Edición no destructiva]]
- [[Flujo de edición de fotos]]
- [[Espacios de color (sRGB, Adobe RGB y Display P3)]]
- [[Calibración del monitor]]

## Laboratorios

- [[Práctica Edición 01 - Prepara tu estación de edición]]

## Progreso

```base
filters:
  and:
    - file.inFolder("32-Edicion-Foto/01 Fundamentos")
    - file.name != "_Índice Fundamentos"
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
