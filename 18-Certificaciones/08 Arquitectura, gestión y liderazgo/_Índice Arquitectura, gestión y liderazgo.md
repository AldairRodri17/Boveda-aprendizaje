---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Certificaciones
tags: [indice, certificaciones]
---
# Arquitectura, gestión y liderazgo

Parte de: [[18-Certificaciones/_Índice Certificaciones|Certificaciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[TOGAF]]
- [[Scrum (PSM y PSPO)]]
- [[Certificaciones de inglés (TOEFL, IELTS, Cambridge y EF SET)]]
- [[Cómo decidir qué certificación vale la pena]]
- [[Plan de certificaciones (calendario y presupuesto)]]
- [[Vouchers y descuentos de examen]]

## Progreso

```base
filters:
  and:
    - file.inFolder("18-Certificaciones/08 Arquitectura, gestión y liderazgo")
    - file.name != "_Índice Arquitectura, gestión y liderazgo"
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
