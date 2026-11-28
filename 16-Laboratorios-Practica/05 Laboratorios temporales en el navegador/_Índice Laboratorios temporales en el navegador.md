---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Laboratorios y práctica
tags: [indice, laboratorios-practica]
contenido: completo
---
# Laboratorios temporales en el navegador

Parte de: [[16-Laboratorios-Practica/_Índice Laboratorios y práctica|Laboratorios y práctica]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Killercoda]]
- [[Play with Docker]]
- [[Play with Kubernetes]]
- [[GitHub Codespaces]]
- [[Learn Git Branching]]

## Progreso

```base
filters:
  and:
    - file.inFolder("16-Laboratorios-Practica/05 Laboratorios temporales en el navegador")
    - file.name != "_Índice Laboratorios temporales en el navegador"
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
