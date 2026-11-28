---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Gestión de personas
tags: [indice, gestion-personas]
---
# Delegar y desarrollar

Parte de: [[53-Gestion-de-Personas/_Índice Gestión de personas|Gestión de personas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Delegar por niveles de autonomía]]
- [[Planes de desarrollo profesional]]
- [[Mentoría y coaching]] ⭐

## Progreso

```base
filters:
  and:
    - file.inFolder("53-Gestion-de-Personas/03 Delegar y desarrollar")
    - file.name != "_Índice Delegar y desarrollar"
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
