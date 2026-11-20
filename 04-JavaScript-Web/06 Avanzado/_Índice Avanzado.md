---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: JavaScript y Web
tags: [indice, javascript-web]
---
# Avanzado

Parte de: [[04-JavaScript-Web/_Índice JavaScript y Web|JavaScript y Web]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Proxy y Reflect]]
- [[Symbol]]
- [[Debounce y throttle]]
- [[Fugas de memoria]]
- [[Rendimiento web y Core Web Vitals]]
  - [[LCP (Largest Contentful Paint)]]
  - [[INP (Interaction to Next Paint)]]
  - [[CLS (Cumulative Layout Shift)]]
- [[SEO y Open Graph]]
- [[Depuración con DevTools]]
- [[npm y package.json]]
- [[pnpm workspaces]]
- [[Vite]]
- [[ESLint y Prettier]]
- [[TypeScript]]
- [[TypeScript - tipos básicos]]
- [[TypeScript - interfaces y types]]
- [[TypeScript - genéricos]]
- [[TypeScript - uniones y narrowing]]
- [[Node.js básico]]
- [[Pruebas con Vitest y Jest]]
- [[Librería de componentes propia en JS plano]]
- [[Design systems]]
- [[Micro-frontends (conceptos)]]

## Laboratorios

- [[Lab 7 - TypeScript básico con un módulo de carrito]]
- [[Lab 8 - Medir Core Web Vitals con DevTools y Lighthouse]]

## Progreso

```base
filters:
  and:
    - file.inFolder("04-JavaScript-Web/06 Avanzado")
    - file.name != "_Índice Avanzado"
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
