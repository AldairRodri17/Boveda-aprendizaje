---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: JavaScript y Web
tags: [indice, javascript-web]
---
# El navegador

Parte de: [[04-JavaScript-Web/_Índice JavaScript y Web|JavaScript y Web]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[DOM - selección y creación]]
- [[DOM - eventos]]
- [[Propagación de eventos (bubbling y capturing)]]
- [[Delegación de eventos]]
- [[Formularios y FormData]]
- [[localStorage]]
- [[History API y navegación]]
- [[SPA y router por hash]]
- [[postMessage e iframes]]
- [[IntersectionObserver y MutationObserver]]
- [[Web Components]]
- [[Shadow DOM]]
- [[Service Workers y PWA]]
- [[Seguridad del DOM (innerHTML y escape de datos)]]
- [[Pipeline de renderizado del navegador]]

## Laboratorios

- [[Lab 5 - Lista de tareas con delegación de eventos]]
- [[Lab 6 - Web Component con Shadow DOM]]

## Progreso

```base
filters:
  and:
    - file.inFolder("04-JavaScript-Web/04 El navegador")
    - file.name != "_Índice El navegador"
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
