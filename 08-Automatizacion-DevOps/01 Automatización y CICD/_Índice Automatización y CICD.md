---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Automatización, DevOps y operación
tags: [indice, automatizacion-devops]
---
# Automatización y CICD

Parte de: [[08-Automatizacion-DevOps/_Índice Automatización, DevOps y operación|Automatización, DevOps y operación]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[CI y CD - conceptos]]
- [[Alternativas para automatizar (comparativa)]]
- [[GitHub Actions]]
- [[Jenkins (conceptos)]]
- [[Infraestructura como código - conceptos]]
- [[Pulumi]]
- [[Scripts con PowerShell]]
- [[Scripts con bash]]
- [[Scripts con Python]]
- [[Docker]]
- [[Docker Compose]]
- [[Kubernetes - conceptos básicos]]
- [[Azure Functions programadas]]
- [[Logic Apps]]
- [[Power Automate]]
- [[n8n]]
- [[Zapier y Make]]
- [[cron y Programador de tareas]]
- Hooks de Git: ver [[Git - hooks y pre-commit]]
- [[Dependabot y Renovate]]
- [[Estrategias de despliegue (blue-green y canary)]]
  - [[Despliegue blue-green]]
  - [[Despliegue canary]]
  - [[Despliegue rolling]]
- [[Rollback y gestión de releases]]
- [[Feature flags]]
- Migraciones: ver [[Migraciones de base de datos (scripts versionados, DACPAC, DbUp y Flyway)]]
- [[Análisis estático (SonarQube y linters)]]
- Pruebas E2E: ver [[Pruebas end-to-end (Playwright y Cypress)]] (sección 13)
- [[Automatización con agentes de IA]]
- Secretos en pipelines: ver [[Manejo de secretos]]
- [[GitOps]]
- [[Métricas DORA]]
  - [[Frecuencia de despliegue (DORA)]]
  - [[Tiempo de entrega de cambios (DORA)]]
  - [[Tasa de fallos de cambios (DORA)]]
  - [[Tiempo de recuperación de despliegues fallidos (DORA)]]

## Laboratorios

- [[Lab 1 - Dockerfile y Docker Compose de una API con SQL Server]]
- [[Lab 2 - GitHub Actions de build y pruebas]]

## Progreso

```base
filters:
  and:
    - file.inFolder("08-Automatizacion-DevOps/01 Automatización y CICD")
    - file.name != "_Índice Automatización y CICD"
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
