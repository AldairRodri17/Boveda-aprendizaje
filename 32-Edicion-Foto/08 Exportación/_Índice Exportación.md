---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de fotografía
tags: [indice, edicion-foto]
---
# Exportación

Parte de: [[32-Edicion-Foto/_Índice Edición de fotografía|Edición de fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Formatos de exportación de imagen (JPEG, PNG, TIFF, HEIC, WebP y AVIF)]]
- [[Tamaños para web y redes sociales]]
- [[Marca de agua]]
- [[Impresión fotográfica (DPI, perfiles y prueba en pantalla)]]

## Laboratorios

- [[Práctica Edición 11 - Exportar para Instagram e impresión]]

## Progreso

```base
filters:
  and:
    - file.inFolder("32-Edicion-Foto/08 Exportación")
    - file.name != "_Índice Exportación"
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
