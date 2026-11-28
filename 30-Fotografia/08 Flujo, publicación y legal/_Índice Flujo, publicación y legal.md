---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fotografía
tags: [indice, fotografia]
contenido: completo
---
# Flujo, publicación y legal

Parte de: [[30-Fotografia/_Índice Fotografía|Fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Importar, nombrar y organizar fotos]]
- [[Respaldo 3-2-1 de fotos y videos]]
- [[Metadatos EXIF e IPTC]]
- [[Derechos de autor de fotos y videos]]
- [[Permiso de modelo y fotografía en espacios públicos en México]]
- [[Portafolio fotográfico y dónde publicar]]
- [[Pedir y dar crítica fotográfica]]

## Laboratorios

- [[Práctica Foto 11 - Importar, organizar y respaldar]]
- [[Práctica Foto 12 - Reto 52 semanas y portafolio]]

## Progreso

```base
filters:
  and:
    - file.inFolder("30-Fotografia/08 Flujo, publicación y legal")
    - file.name != "_Índice Flujo, publicación y legal"
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
