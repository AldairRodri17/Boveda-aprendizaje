---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Domótica y casa inteligente
tags: [indice, domotica]
contenido: completo
---
# Dispositivos

Parte de: [[62-Domotica/_Índice Domótica y casa inteligente|Domótica y casa inteligente]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Enchufes y focos inteligentes]]
- [[Sensores de movimiento, puerta, temperatura y humedad]] ⭐
- [[Cámaras de seguridad con grabación local]]
- [[Cerraduras y timbres inteligentes]]
- [[Asistentes de voz]]

## Progreso

```base
filters:
  and:
    - file.inFolder("62-Domotica/03 Dispositivos")
    - file.name != "_Índice Dispositivos"
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
