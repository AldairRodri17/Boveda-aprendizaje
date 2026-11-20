---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fundamentos
tags: [indice, fundamentos]
---
# Internet y web

Parte de: [[02-Fundamentos/_Índice Fundamentos|Fundamentos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cliente y servidor]]
- [[Dirección IP y puertos]]
- [[DNS]]
- [[TCP y UDP]]
- [[Anatomía de una URL]]
- [[HTTP vs HTTPS]]
- [[TLS y certificados]]
- [[HTTP 1.1, HTTP 2 y HTTP 3]]
- [[Proxy, proxy inverso y balanceador de carga]]
- [[CDN]]
- [[Latencia, ancho de banda y timeouts]]

## Laboratorios

- [[Lab 1 - DNS, TLS y tiempos de red con curl]]

## Progreso

```base
filters:
  and:
    - file.inFolder("02-Fundamentos/01 Internet y web")
    - file.name != "_Índice Internet y web"
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
