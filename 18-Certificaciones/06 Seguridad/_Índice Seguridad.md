---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Certificaciones
tags: [indice, certificaciones]
contenido: completo
---
# Seguridad

Parte de: [[18-Certificaciones/_Índice Certificaciones|Certificaciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Burp Suite Certified Practitioner (PortSwigger)]]
- [[CompTIA Security+]]
- [[eJPT]]
- [[CEH]]
- [[OSCP]]
- [[Certified in Cybersecurity (ISC2)]]
- [[SC-200 Security Operations Analyst]]

## Progreso

```base
filters:
  and:
    - file.inFolder("18-Certificaciones/06 Seguridad")
    - file.name != "_Índice Seguridad"
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
