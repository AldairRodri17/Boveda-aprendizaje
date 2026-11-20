---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Gestión de personas
tags: [indice, gestion-personas]
---
# El rol de líder

Parte de: [[53-Gestion-de-Personas/_Índice Gestión de personas|Gestión de personas]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[De desarrollador a líder]] ⭐
- [[Tech lead - responsabilidades]] (vive en 14 · Carrera)
- [[Estilos de liderazgo]]

## Progreso

```base
filters:
  and:
    - file.inFolder("53-Gestion-de-Personas/01 El rol de líder")
    - file.name != "_Índice El rol de líder"
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
