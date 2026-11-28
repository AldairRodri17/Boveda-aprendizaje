---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Integraciones
tags: [indice, integraciones]
contenido: completo
---
# Documentos y archivos

Parte de: [[22-Integraciones/_Índice Integraciones|Integraciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Generar documentos Word desde .NET (Open XML)]]
- [[Generar Excel desde .NET]]
- [[ImageSharp]]

## Laboratorios

- [[Lab 3 - Generar un .docx y un .xlsx desde .NET]]

## Progreso

```base
filters:
  and:
    - file.inFolder("22-Integraciones/03 Documentos y archivos")
    - file.name != "_Índice Documentos y archivos"
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
