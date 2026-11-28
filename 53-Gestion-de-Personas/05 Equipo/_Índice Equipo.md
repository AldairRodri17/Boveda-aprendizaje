---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Gestión de personas
tags: [indice, gestion-personas]
contenido: completo
---
# Equipo

Parte de: [[53-Gestion-de-Personas/_Índice Gestión de personas|Gestión de personas]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Seguridad psicológica]] ⭐
- [[Rituales de equipo (retros, planning y demos)]]
- [[Conflictos en el equipo]]
- [[Onboarding de nuevos integrantes]]

## Progreso

```base
filters:
  and:
    - file.inFolder("53-Gestion-de-Personas/05 Equipo")
    - file.name != "_Índice Equipo"
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
