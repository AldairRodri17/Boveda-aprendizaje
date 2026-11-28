---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: JavaScript y Web
tags: [indice, javascript-web]
contenido: completo
---
# HTML y CSS

Parte de: [[04-JavaScript-Web/_Índice JavaScript y Web|JavaScript y Web]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[HTML semántico]]
- [[Formularios HTML]]
- [[Elemento dialog]]
- [[Accesibilidad web]]
  - [[Roles y atributos ARIA]]
  - [[Navegación con teclado y foco]]
  - [[Contraste y texto alternativo]]
- [[CSS - selectores y especificidad]]
- [[CSS - box model]]
- [[CSS - flexbox]]
- [[CSS - grid]]
- [[CSS - diseño responsivo]]
- [[Imágenes responsive]]
- [[CSS - variables]]
- [[CSS - animaciones y transiciones]]
- [[CSS - metodologías (BEM)]]

## Laboratorios

- [[Lab 1 - Página de registro accesible y responsiva]]

## Progreso

```base
filters:
  and:
    - file.inFolder("04-JavaScript-Web/01 HTML y CSS")
    - file.name != "_Índice HTML y CSS"
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
