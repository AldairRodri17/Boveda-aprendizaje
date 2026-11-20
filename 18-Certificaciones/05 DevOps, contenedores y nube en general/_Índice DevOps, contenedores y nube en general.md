---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Certificaciones
tags: [indice, certificaciones]
---
# DevOps, contenedores y nube en general

Parte de: [[18-Certificaciones/_Índice Certificaciones|Certificaciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[GH-900 GitHub Foundations]]
- [[GH-200 GitHub Actions]]
- [[Terraform Associate]]
- [[Kubernetes (KCNA, CKAD y CKA)]]
- [[AWS Cloud Practitioner]]
- [[AWS Solutions Architect Associate]]
- [[Google Cloud Professional Cloud Architect]]

## Progreso

```base
filters:
  and:
    - file.inFolder("18-Certificaciones/05 DevOps, contenedores y nube en general")
    - file.name != "_Índice DevOps, contenedores y nube en general"
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
