---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Certificaciones
tags: [indice, certificaciones]
---
# Desarrollo

Parte de: [[18-Certificaciones/_Índice Certificaciones|Certificaciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Certificaciones de CSharp y .NET]]
- [[Certificaciones de JavaScript y Node.js]]
- [[React - por qué no hay certificación oficial y qué hacer en su lugar]]
- [[Angular - certificaciones y cómo demostrar el nivel]]
- [[Certificaciones de SQL y bases de datos]]
- [[Oracle Certified Professional Java SE]]
- [[Spring Professional]]

## Progreso

```base
filters:
  and:
    - file.inFolder("18-Certificaciones/07 Desarrollo")
    - file.name != "_Índice Desarrollo"
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
