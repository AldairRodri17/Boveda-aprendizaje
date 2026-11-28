---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Seguridad
tags: [indice, seguridad]
contenido: completo
---
# XSS

Parte de: [[12-Seguridad/_Índice Seguridad|Seguridad]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[XSS]]
- [[XSS - qué es y por qué ocurre]]
- [[XSS - reflejado]]
- [[XSS - almacenado]]
- [[XSS - basado en DOM]]
- [[XSS - contextos de salida (HTML, atributo, JavaScript y URL)]]
- [[XSS - impacto real]]
- [[XSS - codificación de salida según el contexto]]
- [[XSS - textContent vs innerHTML]]
- [[XSS - escape de datos del API en templates de JS]]
- [[XSS - sanitización de HTML (DOMPurify)]]
- [[XSS - protección por defecto de React y dangerouslySetInnerHTML]]
- [[XSS - Razor y HtmlEncoder en ASP.NET Core]]
- [[Content Security Policy]]
- [[Trusted Types]]
- HttpOnly como mitigación: ver [[Cookies - Secure y HttpOnly]]
- [[XSS - cómo detectarlo]]

## Laboratorios

- [[Lab 5 - CSP y codificación de salida en una página propia]]
- [[Lab 6 - PortSwigger - laboratorios de XSS nivel Apprentice]]

## Progreso

```base
filters:
  and:
    - file.inFolder("12-Seguridad/03 XSS")
    - file.name != "_Índice XSS"
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
