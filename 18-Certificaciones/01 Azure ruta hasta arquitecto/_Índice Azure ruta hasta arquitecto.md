---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Certificaciones
tags: [indice, certificaciones]
contenido: completo
---
# Azure ruta hasta arquitecto

Parte de: [[18-Certificaciones/_Índice Certificaciones|Certificaciones]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Mapa de certificaciones de Azure y cómo elegir]]
- [[AZ-900 Azure Fundamentals (opcional)]]
- [[AZ-104 Azure Administrator Associate]]
- [[AZ-204 Azure Developer Associate (retirado el 31 de julio de 2026)]]
- [[AI-200 Azure AI Cloud Developer Associate (sucesor de AZ-204)]]
- [[AZ-400 DevOps Engineer Expert]]
- [[AZ-305 Azure Solutions Architect Expert]]
- [[Requisitos y orden recomendado hasta Azure Solutions Architect]]
- [[AZ-500 y SC-500 (seguridad en Azure)]]
- [[DP-300 Azure Database Administrator]]
- [[SC-900 Security, Compliance and Identity Fundamentals]]
- [[SC-100 Cybersecurity Architect Expert]]
- [[Renovación anual de certificaciones de Microsoft]]
- [[Cómo estudiar con Microsoft Learn y evaluaciones de práctica]]
- [[Cómo prepararse para los casos de estudio de AZ-305]]

## Progreso

```base
filters:
  and:
    - file.inFolder("18-Certificaciones/01 Azure ruta hasta arquitecto")
    - file.name != "_Índice Azure ruta hasta arquitecto"
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
