---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Edición de fotografía
tags: [indice, edicion-foto]
contenido: completo
---
# Edición de fotografía

Avance y prioridades en [[Temario]]. Mapa visual: [[Mapa mental - Edición de fotografía.canvas|Mapa mental]].

## Secciones

- [[32-Edicion-Foto/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[32-Edicion-Foto/02 Programas gratuitos/_Índice Programas gratuitos|Programas gratuitos]]
- [[32-Edicion-Foto/03 Revelado/_Índice Revelado|Revelado]]
- [[32-Edicion-Foto/04 Ajustes locales y retoque/_Índice Ajustes locales y retoque|Ajustes locales y retoque]]
- [[32-Edicion-Foto/05 Capas y composición/_Índice Capas y composición|Capas y composición]]
- [[32-Edicion-Foto/06 Look y técnicas/_Índice Look y técnicas|Look y técnicas]]
- [[32-Edicion-Foto/07 IA en la edición/_Índice IA en la edición|IA en la edición]]
- [[32-Edicion-Foto/08 Exportación/_Índice Exportación|Exportación]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("32-Edicion-Foto")
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
