---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Salud financiera
tags: [indice, finanzas]
---
# Salud financiera

Avance y prioridades en [[Temario]].

> [!note]
> Contenido educativo, no asesoría financiera. Cifras, topes y reglas (SAT, CONDUSEF, Banxico, CONSAR, IMSS) se verifican en la fuente oficial al escribir cada nota.

## Secciones

- [[41-Finanzas-Personales/01 Fundamentos/_Índice Fundamentos|Fundamentos]]
- [[41-Finanzas-Personales/02 Presupuesto/_Índice Presupuesto|Presupuesto]]
- [[41-Finanzas-Personales/03 Fondo de emergencia/_Índice Fondo de emergencia|Fondo de emergencia]]
- [[41-Finanzas-Personales/04 Crédito y deudas/_Índice Crédito y deudas|Crédito y deudas]]
- [[41-Finanzas-Personales/05 Seguros/_Índice Seguros|Seguros]]
- [[41-Finanzas-Personales/06 Retiro/_Índice Retiro|Retiro]]
- [[41-Finanzas-Personales/07 Vivienda/_Índice Vivienda|Vivienda]]
- [[41-Finanzas-Personales/08 Impuestos/_Índice Impuestos|Impuestos]]
- [[41-Finanzas-Personales/09 Protección/_Índice Protección|Protección]]

## Progreso de toda el área

```base
filters:
  and:
    - file.inFolder("41-Finanzas-Personales")
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
