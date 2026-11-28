---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Mitología e historia mesoamericana y griega
tags: [indice, mitologia]
contenido: completo
---
# Mexicas

Parte de: [[67-Mitologia/_Índice Mitología e historia mesoamericana y griega|Mitología e historia mesoamericana y griega]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Los mexicas - historia y Tenochtitlan]] ⭐
- [[Dioses mexicas principales]] ⭐
- [[Quetzalcóatl]]
- [[Huitzilopochtli]]
- [[Tezcatlipoca]]
- [[Tláloc]]
- [[Mito de los cinco soles]] ⭐
- [[Los pochtecas (comerciantes mexicas)]] ⭐ 🛠 (Pochteca.Nomisma)
- [[Ocelotl y los guerreros jaguar]] 🛠 (Ocelotl)

## Progreso

```base
filters:
  and:
    - file.inFolder("67-Mitologia/02 Mexicas")
    - file.name != "_Índice Mexicas"
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
