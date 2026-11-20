---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Homelab y self-hosting
tags: [indice, homelab]
---
# Almacenamiento

Parte de: [[61-Homelab/_Índice Homelab y self-hosting|Homelab y self-hosting]] · Avance general en [[Temario v2 - senior y arquitecto]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[NAS (conceptos y opciones)]] ⭐
- [[TrueNAS y Unraid]]
- [[RAID y ZFS]] ⭐
- [[Discos para NAS]]

## Progreso

```base
filters:
  and:
    - file.inFolder("61-Homelab/03 Almacenamiento")
    - file.name != "_Índice Almacenamiento"
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
