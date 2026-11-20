---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Restauración del Vocho
tags: [indice, vocho]
---
# Sistema eléctrico

Parte de: [[68-Restauracion-Vocho/_Índice Restauración del Vocho|Restauración del Vocho]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Sistema eléctrico del vocho]] ⭐
- [[Batería, generador o alternador]]
- [[Luces, fusibles y arnés]]
- [[Leer el diagrama eléctrico del vocho]] (enlaza con 63)

## Progreso

```base
filters:
  and:
    - file.inFolder("68-Restauracion-Vocho/06 Sistema eléctrico")
    - file.name != "_Índice Sistema eléctrico"
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
