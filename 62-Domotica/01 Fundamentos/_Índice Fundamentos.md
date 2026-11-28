---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Domótica y casa inteligente
tags: [indice, domotica]
---
# Fundamentos

Parte de: [[62-Domotica/_Índice Domótica y casa inteligente|Domótica y casa inteligente]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es la domótica]] ⭐
- [[Protocolos de domótica (Wi-Fi, Zigbee, Z-Wave, Thread y Matter)]] ⭐
- [[Control en la nube vs control local]]
- [[Seguridad y privacidad en la casa inteligente]]

## Progreso

```base
filters:
  and:
    - file.inFolder("62-Domotica/01 Fundamentos")
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
