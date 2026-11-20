---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fotografía
tags: [indice, fotografia]
---
# Luz y color

Parte de: [[30-Fotografia/_Índice Fotografía|Fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Balance de blancos y temperatura de color]]
- [[Luz dura y luz suave]]
- [[Dirección de la luz]]
- [[Hora dorada y hora azul]]
- [[Teoría del color y armonías]]
- [[Flash (TTL, manual, rebotado y alta velocidad)]]
- [[Modificadores de luz (difusor, reflector y softbox)]]
- [[Esquemas de iluminación de retrato]]
  - [[Iluminación Rembrandt]]
  - [[Iluminación loop]]
  - [[Iluminación split]]
  - [[Iluminación mariposa]]
  - [[Iluminación clamshell]]

## Laboratorios

- [[Práctica Foto 05 - Retrato con luz de ventana y reflector casero]]
- [[Práctica Foto 06 - Balance de blancos y horas de luz]]

## Progreso

```base
filters:
  and:
    - file.inFolder("30-Fotografia/04 Luz y color")
    - file.name != "_Índice Luz y color"
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
