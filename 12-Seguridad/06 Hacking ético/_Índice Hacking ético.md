---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Seguridad
tags: [indice, seguridad]
---
# Hacking ético

Parte de: [[12-Seguridad/_Índice Seguridad|Seguridad]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es el hacking ético]]
- [[Marco legal y autorización por escrito]]
- [[Alcance (scope) de una prueba de seguridad]]
- [[Qué nunca hacer]]
- [[Divulgación responsable]]
- [[Programas de bug bounty]]
- [[CVE y CVSS]]
- [[Cómo escribir un reporte de vulnerabilidad]]
- [[Metodología de pruebas web (OWASP WSTG)]]
- [[Pentesting vs red team]]

## Laboratorios

- [[Lab 11 - Carta de autorización y reporte de vulnerabilidad]]

## Progreso

```base
filters:
  and:
    - file.inFolder("12-Seguridad/06 Hacking ético")
    - file.name != "_Índice Hacking ético"
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
