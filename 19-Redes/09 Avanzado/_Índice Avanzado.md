---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Redes
tags: [indice, redes]
---
# Avanzado

Parte de: [[19-Redes/_Índice Redes|Redes]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[WAN y SD-WAN]]
- [[SDN (redes definidas por software)]]
- [[Automatización de redes (Python, Netmiko y Ansible)]]
- [[NETCONF y RESTCONF]]
- [[Redes en la nube (VNet, subnets, NSG y peering)]]
- [[Redes de contenedores (Docker y Kubernetes)]]

## Progreso

```base
filters:
  and:
    - file.inFolder("19-Redes/09 Avanzado")
    - file.name != "_Índice Avanzado"
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
