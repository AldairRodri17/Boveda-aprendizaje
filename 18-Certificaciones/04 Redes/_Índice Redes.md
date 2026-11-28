---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Certificaciones
tags: [indice, certificaciones]
contenido: completo
---
# Redes

Parte de: [[18-Certificaciones/_Índice Certificaciones|Certificaciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cisco Networking Academy (cursos y Packet Tracer)]]
- [[Cisco CCST Networking]]
- [[CCNA]]
- [[CCNP Enterprise]]
- [[CompTIA Network+]]

## Progreso

```base
filters:
  and:
    - file.inFolder("18-Certificaciones/04 Redes")
    - file.name != "_Índice Redes"
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
