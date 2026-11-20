---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Seguridad
tags: [indice, seguridad]
---
# Herramientas defensivas

Parte de: [[12-Seguridad/_Índice Seguridad|Seguridad]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[SAST, DAST y SCA]]
- [[Escaneo de dependencias y de secretos]]
- [[OWASP ZAP]]
- [[Burp Suite Community]]
- [[Pruebas de seguridad en el pipeline]]
- [[WAF]]

## Laboratorios

- [[Lab 9 - Escaneo de dependencias y secretos en un repositorio propio]]
- [[Lab 10 - Escaneo pasivo con OWASP ZAP contra Juice Shop local]]

## Progreso

```base
filters:
  and:
    - file.inFolder("12-Seguridad/05 Herramientas defensivas")
    - file.name != "_Índice Herramientas defensivas"
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
