---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Azure
tags: [indice, azure]
contenido: completo
---
# Fundamentos

Parte de: [[07-Azure/_Índice Azure|Azure]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Azure - suscripciones, grupos de recursos y regiones]]
- [[Azure - Resource Manager]]
- [[Azure - etiquetas (tags)]]
- [[Azure - costos y Cost Management]]
- [[Azure - capas gratuitas]]
- [[Azure - RBAC]]
- [[Azure - Entra ID y registro de aplicaciones]]
- [[Azure CLI]]
- [[Azure Portal vs CLI vs IaC]]
- [[Azure Well-Architected Framework]]
  - [[Well-Architected - Confiabilidad]]
  - [[Well-Architected - Seguridad]]
  - [[Well-Architected - Optimización de costos]]
  - [[Well-Architected - Excelencia operativa]]
  - [[Well-Architected - Eficiencia del rendimiento]]
- Certificaciones: ver [[Mapa de certificaciones de Azure y cómo elegir]] (sección 18)

## Laboratorios

- [[Lab 1 - Crear y borrar recursos con Azure CLI en el sandbox]]

## Progreso

```base
filters:
  and:
    - file.inFolder("07-Azure/01 Fundamentos")
    - file.name != "_Índice Fundamentos"
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
