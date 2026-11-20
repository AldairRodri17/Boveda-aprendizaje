---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
---
# Servicios de red

Parte de: [[19-Redes/_Índice Redes|Redes]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[DHCP]]
  - [[Proceso DORA de DHCP]]
  - [[DHCP relay]]
- [[DNS en la red (resolución, registros y servidores)]]
- [[NAT y PAT]]
  - [[NAT estático]]
  - [[NAT dinámico]]
  - [[PAT (sobrecarga de NAT)]]
- [[NTP]]
- [[SNMP y Syslog]]
- [[FTP, SFTP y TFTP]]
- [[QoS (calidad de servicio)]]

## Laboratorios

- [[Lab 1 - DHCP y NAT-PAT en Packet Tracer]]

## Progreso

```base
filters:
  and:
    - file.inFolder("19-Redes/04 Servicios de red")
    - file.name != "_Índice Servicios de red"
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
