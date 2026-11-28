---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
contenido: completo
---
# Cisco IOS y práctica

Parte de: [[19-Redes/_Índice Redes|Redes]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Cisco Packet Tracer]]
- [[Cisco IOS - modos y comandos básicos]]
- [[Configuración inicial de un switch Cisco]]
- [[Configuración inicial de un router Cisco]]
- [[SSH y acceso seguro a dispositivos]]
- [[Respaldo de configuraciones]]
- [[Wireshark (captura y análisis de tráfico)]]
- [[Troubleshooting de redes (metodología por capas)]]
- [[Laboratorios de Packet Tracer (bitácora)]]

## Laboratorios

- [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]]
- [[Lab 2 - Wireshark capturando ARP, ICMP y DNS]]

## Progreso

```base
filters:
  and:
    - file.inFolder("19-Redes/08 Cisco IOS y práctica")
    - file.name != "_Índice Cisco IOS y práctica"
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
