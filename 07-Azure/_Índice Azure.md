---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Azure
tags: [indice, azure]
---
# Azure

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Azure.canvas|Mapa mental]].

## Secciones

- [[07-Azure/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[07-Azure/02 Dónde probar Azure sin pagar/_Índice Dónde probar Azure sin pagar|Dónde probar Azure sin pagar]]
- [[07-Azure/03 Cómputo/_Índice Cómputo|Cómputo]]
- [[07-Azure/04 Datos/_Índice Datos|Datos]]
- [[07-Azure/05 Integración/_Índice Integración|Integración]]
- [[07-Azure/06 Seguridad/_Índice Seguridad|Seguridad]]
- [[07-Azure/07 Redes en Azure/_Índice Redes en Azure|Redes en Azure]]
- [[07-Azure/08 Observabilidad/_Índice Observabilidad|Observabilidad]]
- [[07-Azure/09 Azure DevOps/_Índice Azure DevOps|Azure DevOps]]
- [[07-Azure/10 Infraestructura como código/_Índice Infraestructura como código|Infraestructura como código]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("07-Azure")
    - tipo != "indice"
views:
  - type: table
    name: Pendientes
    filters:
      and:
        - progreso != "visto"
        - progreso != "dominado"
    order:
      - file.name
      - file.folder
      - progreso
      - nivel
  - type: table
    name: Vistos
    filters:
      and:
        - progreso == "visto"
    order:
      - file.name
      - file.folder
  - type: table
    name: Dominados
    filters:
      and:
        - progreso == "dominado"
    order:
      - file.name
      - file.folder
```
