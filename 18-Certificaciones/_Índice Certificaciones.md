---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Certificaciones
tags: [indice, certificaciones]
---
# Certificaciones

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Certificaciones.canvas|Mapa mental]].

## Secciones

- [[18-Certificaciones/01 Azure ruta hasta arquitecto/_Índice Azure ruta hasta arquitecto|Azure ruta hasta arquitecto]]
- [[18-Certificaciones/02 IA y datos/_Índice IA y datos|IA y datos]]
- [[18-Certificaciones/03 Power BI y datos/_Índice Power BI y datos|Power BI y datos]]
- [[18-Certificaciones/04 Redes/_Índice Redes|Redes]]
- [[18-Certificaciones/05 DevOps, contenedores y nube en general/_Índice DevOps, contenedores y nube en general|DevOps, contenedores y nube en general]]
- [[18-Certificaciones/06 Seguridad/_Índice Seguridad|Seguridad]]
- [[18-Certificaciones/07 Desarrollo/_Índice Desarrollo|Desarrollo]]
- [[18-Certificaciones/08 Arquitectura, gestión y liderazgo/_Índice Arquitectura, gestión y liderazgo|Arquitectura, gestión y liderazgo]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("18-Certificaciones")
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
