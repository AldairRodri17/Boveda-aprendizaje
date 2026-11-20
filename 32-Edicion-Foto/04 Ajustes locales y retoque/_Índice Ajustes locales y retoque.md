---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de fotografía
tags: [indice, edicion-foto]
---
# Ajustes locales y retoque

Parte de: [[32-Edicion-Foto/_Índice Edición de fotografía|Edición de fotografía]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Máscaras y ajustes locales]]
- [[Dodge and burn]]
- [[Clonar y parchar]]
- [[Retoque de piel con separación de frecuencias]]
- [[Ética del retoque]]

## Laboratorios

- [[Práctica Edición 05 - Máscaras locales y dodge and burn]]
- [[Práctica Edición 06 - Limpieza y retoque de piel]]

## Progreso

```base
filters:
  and:
    - file.inFolder("32-Edicion-Foto/04 Ajustes locales y retoque")
    - file.name != "_Índice Ajustes locales y retoque"
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
