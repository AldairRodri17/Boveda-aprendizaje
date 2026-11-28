---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Homelab y self-hosting
tags: [indice, homelab]
contenido: completo
---
# Fundamentos

Parte de: [[61-Homelab/_Índice Homelab y self-hosting|Homelab y self-hosting]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es un homelab y para qué sirve]] ⭐
- [[Hardware para un homelab (mini PC, PC vieja y Raspberry Pi)]] ⭐
- [[Consumo eléctrico y ruido del homelab]]
- [[Planear tu homelab]]

## Progreso

```base
filters:
  and:
    - file.inFolder("61-Homelab/01 Fundamentos")
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
