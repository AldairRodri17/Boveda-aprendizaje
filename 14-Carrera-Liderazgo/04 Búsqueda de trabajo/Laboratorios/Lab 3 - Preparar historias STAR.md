---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Carrera y liderazgo técnico
herramienta: Obsidian y grabadora de voz
tags: [carrera-liderazgo, laboratorio]
---
# Lab 3 - Preparar historias STAR

Parte de: [[14-Carrera-Liderazgo/04 Búsqueda de trabajo/_Índice Búsqueda de trabajo|Búsqueda de trabajo]]

**Cubre:** [[Entrevistas de comportamiento (método STAR)]] · [[Preparación para entrevistas técnicas]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Tener listas 6 historias STAR reales que cubran las preguntas de comportamiento más comunes, ensayadas en voz alta (en español y al menos 2 en inglés).

## Pasos
1. Lista 8–10 experiencias reales: un logro técnico, un conflicto, un error, un plazo imposible, una decisión difícil, liderar sin autoridad, ayudar a alguien, aprender algo rápido.
2. Elige las 6 más fuertes y escribe cada una en formato STAR (máx. 200 palabras), con el **resultado medible**.
3. Marca qué preguntas responde cada historia (una historia puede servir para varias).
4. Grábate contando cada historia en 2 minutos. Escucha y recorta lo que sobre en S y T.
5. Traduce y ensaya 2 historias en inglés.
6. Pide a alguien que te haga 3 preguntas al azar de la lista de abajo y responde con tus historias.

Preguntas de práctica: *Tell me about a time you disagreed with a teammate*; un error y qué aprendiste; un proyecto del que estés orgulloso; cuando tuviste que entregar con poco tiempo; cuando convenciste a otros de un cambio técnico.

## Criterios de terminado
- [ ] 6 historias escritas en STAR, cada una con resultado medible o concreto.
- [ ] Cada historia dura entre 1.5 y 2.5 minutos al contarla.
- [ ] En la parte A se usa "yo" y se describen acciones concretas tuyas.
- [ ] 2 historias ensayadas en inglés.
- [ ] Las 5 preguntas de práctica tienen al menos una historia asignada.

## Solución

> [!success]- Ejemplo completo (conflicto)
> **S:** En un SaaS multi-tenant, un compañero quería quitar los filtros globales por tenant porque complicaban unas consultas de reportes.
> **T:** Yo era responsable del módulo de datos y tenía que resolverlo sin frenar los reportes.
> **A:** Le pedí 30 minutos para entender sus consultas, le mostré con una prueba cómo quitar el filtro exponía datos de otro tenant, y propuse un repositorio de reportes que usa `IgnoreQueryFilters()` solo con el tenant explícito y cubierto por pruebas. Lo documentamos en un ADR.
> **R:** Los reportes salieron a tiempo, no hubo fuga de datos y el ADR se usó después para otros casos parecidos. Aprendí a discutir con pruebas en lugar de opiniones.

> [!success]- Mapa de historias a preguntas
> | Historia | Conflicto | Error | Orgullo | Plazo | Influir |
> |---|---|---|---|---|---|
> | Filtros multi-tenant | X |  |  |  | X |
> | Migración que bloqueó tablas |  | X |  |  |  |
> | Outbox en pagos |  |  | X | X |  |

## Notas personales

_Qué me costó, errores que cometí, dudas._
