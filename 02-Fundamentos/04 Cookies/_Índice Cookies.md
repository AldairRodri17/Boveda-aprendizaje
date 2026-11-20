---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fundamentos
tags: [indice, fundamentos]
---
# Cookies

Parte de: [[02-Fundamentos/_Índice Fundamentos|Fundamentos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cookies]]
- [[Set-Cookie y el header Cookie]]
- [[Cookies - Domain y Path]]
- [[Cookies - Expires y Max-Age]]
- [[Cookies - Secure y HttpOnly]]
- [[Cookies - SameSite]]
- [[Cookies de sesión y persistentes]]
- [[Cookies de primera y tercera parte]]
- [[Cookies entre sistemas propios (subdominios y dominios distintos)]]
- [[Mismo origen, mismo sitio y mismo dominio]]
- [[Cookies y CORS con credenciales]]
- [[Cookies y SSO]]
- [[Cookies vs localStorage vs sessionStorage]]
- [[Cookies vs tokens]]
- [[Cookies y CSRF]]
- [[Cookies en ASP.NET Core (autenticación y Data Protection)]]
- [[Cookies desde JavaScript]]
- [[Restricciones a cookies de terceros en navegadores]]

## Laboratorios

- [[Lab 1 - Cookies y CORS en el navegador con DevTools]]

## Progreso

```base
filters:
  and:
    - file.inFolder("02-Fundamentos/04 Cookies")
    - file.name != "_Índice Cookies"
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
