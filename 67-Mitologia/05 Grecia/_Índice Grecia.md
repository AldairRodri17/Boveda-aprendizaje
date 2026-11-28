---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Mitología e historia mesoamericana y griega
tags: [indice, mitologia]
---
# Grecia

Parte de: [[67-Mitologia/_Índice Mitología e historia mesoamericana y griega|Mitología e historia mesoamericana y griega]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Mitología griega - origen y fuentes]] ⭐
- [[Los dioses del Olimpo]] ⭐
- [[Hermes]] 🛠 (Ocelotl.Hermes)
- [[Iris (mensajera de los dioses)]] 🛠 (Ocelotl.Iris)
- [[Hécate]] 🛠 (Hecate)
- [[Dionisio]] 🛠 (Itzamna.Dionisio)
- [[Las musas y Clío]] 🛠 (Itzamna.Clio)
- [[Argo y los argonautas]] 🛠 (Argo)
- [[Nomisma (la moneda griega)]] 🛠 (Pochteca.Nomisma)
- [[Héroes griegos (Heracles, Perseo y Teseo)]]
- [[La Ilíada y la Odisea]] ⭐

## Progreso

```base
filters:
  and:
    - file.inFolder("67-Mitologia/05 Grecia")
    - file.name != "_Índice Grecia"
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
