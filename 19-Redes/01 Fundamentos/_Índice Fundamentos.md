---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
contenido: completo
---
# Fundamentos

Parte de: [[19-Redes/_Índice Redes|Redes]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Qué es una red (LAN, WAN, MAN, PAN)]]
- [[Topologías de red]]
- [[Modelo OSI]]
  - [[Capa 1 - Física]]
  - [[Capa 2 - Enlace de datos]]
  - [[Capa 3 - Red]]
  - [[Capa 4 - Transporte]]
  - [[Capa 5 - Sesión]]
  - [[Capa 6 - Presentación]]
  - [[Capa 7 - Aplicación]]
- [[Modelo TCP-IP]]
- [[Encapsulamiento y PDUs]]
- [[Medios físicos (cobre, fibra e inalámbrico)]]
  - [[Cable de cobre (UTP y STP)]]
  - [[Fibra óptica (monomodo y multimodo)]]
  - [[Medios inalámbricos]]
- [[Cableado estructurado y estándares (T568A y T568B)]]
- [[Dispositivos de red (hub, switch, router, access point, firewall)]]
  - [[Hub]]
  - [[Switch]]
  - [[Router]]
  - [[Access point]]
  - [[Firewall]]
- [[Hub vs switch vs router]]
- [[Ethernet y tramas]]
- [[Direcciones MAC]]
- [[Dominios de colisión y de broadcast]]

## Laboratorios

- [[Lab 1 - Encapsulamiento, hub y switch en modo simulación]]

## Progreso

```base
filters:
  and:
    - file.inFolder("19-Redes/01 Fundamentos")
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
