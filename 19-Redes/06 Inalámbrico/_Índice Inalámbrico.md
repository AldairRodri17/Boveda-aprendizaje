---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
---
# Inalámbrico

Parte de: [[19-Redes/_Índice Redes|Redes]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Wi-Fi - estándares y bandas]]
- [[WLAN - controladores y access points]]
- [[Seguridad Wi-Fi (WPA2, WPA3 y 802.1X)]]

## Laboratorios

- [[Lab 1 - Wi-Fi básico en Packet Tracer]]

## Progreso

```base
filters:
  and:
    - file.inFolder("19-Redes/06 Inalámbrico")
    - file.name != "_Índice Inalámbrico"
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
