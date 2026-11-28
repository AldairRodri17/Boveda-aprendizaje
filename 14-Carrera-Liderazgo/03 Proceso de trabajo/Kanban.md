---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Carrera y liderazgo técnico
tags: [carrera-liderazgo]
contenido: completo
---
# Kanban

Parte de: [[14-Carrera-Liderazgo/03 Proceso de trabajo/_Índice Proceso de trabajo|Proceso de trabajo]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Kanban** es una estrategia para gestionar el **flujo** de trabajo: se visualiza cada elemento en un tablero, se **limita el trabajo en progreso** (WIP) y se mejora con métricas. A diferencia de Scrum, no tiene sprints ni roles obligatorios; el trabajo entra cuando hay capacidad. La *Kanban Guide* de kanbanguides.org describe la versión centrada en flujo.

### Cómo funciona
Prácticas centrales:
1. **Definir y visualizar el flujo:** columnas que reflejan el proceso real (Por hacer → En desarrollo → En revisión → En QA → Hecho) y reglas claras de cuándo una tarjeta pasa de una a otra.
2. **Limitar el WIP:** cada columna o el tablero completo tiene un máximo de elementos. Si está lleno, no se empieza algo nuevo; se ayuda a terminar.
3. **Gestionar activamente el flujo:** atender lo que está bloqueado o envejeciendo.
4. **Mejorar con métricas:** WIP, *throughput* (elementos terminados por periodo), *cycle time* (cuánto tarda un elemento desde que empieza hasta que termina) y edad de los elementos en curso.

```mermaid
flowchart LR
    A[Por hacer] --> B[Desarrollo<br/>WIP 3]
    B --> C[Revisión<br/>WIP 2]
    C --> D[QA<br/>WIP 2]
    D --> E[Hecho]
```

**¿Para qué el límite de WIP?** Evita "empezar mucho y terminar poco". Obliga a terminar antes de tomar lo siguiente y **expone los cuellos de botella**: si la columna de Revisión siempre está llena, el problema es la revisión, no el desarrollo.

**Ley de Little** (en un sistema estable): `cycle time promedio = WIP promedio / throughput promedio`. Si reduces el WIP sin cambiar el throughput, el trabajo sale más rápido.

### Ejemplo
Equipo de soporte y mantenimiento de Wee.Reporteria:

- Hay **12 tarjetas en progreso** y el equipo termina **4 por semana**.
- Cycle time promedio = 12 / 4 = **3 semanas**.

Es decir, un reporte que entra hoy tardará unas 3 semanas en salir. Si el equipo baja el WIP a 6 tarjetas manteniendo 4 por semana, el cycle time baja a 1.5 semanas sin trabajar más horas: solo dejan de empezar cosas que no pueden terminar.

En Azure Boards puedes configurar límites de WIP por columna; la columna se marca en rojo cuando se excede. Una regla de equipo útil: "Si Revisión está llena, el que termina una tarea revisa un PR antes de tomar otra".

### Cuándo usarlo y cuándo no
Kanban encaja mejor que Scrum cuando el trabajo **llega de forma continua e impredecible**: soporte, operación, mantenimiento, equipos de plataforma, incidentes. También para equipos que ya hacen Scrum y quieren mejorar el flujo dentro del sprint. Si el producto necesita ciclos de planeación y demos periódicas con el negocio, [[Scrum en la práctica]] da esa cadencia (aunque Kanban puede tener reuniones de revisión periódicas). Para pronosticar fechas con Kanban, usa el throughput histórico en lugar de estimar cada tarjeta ([[Estimación de tareas]]).

### Errores comunes
1. **Tablero sin límites de WIP.** Es solo una lista visual; no cambia nada.
2. **Columnas que no reflejan el proceso real** (por ejemplo, no hay columna de revisión y los PRs esperan escondidos en "En desarrollo").
3. **Ignorar tarjetas bloqueadas o viejas.** La edad de los elementos es una alerta temprana.
4. **Subir el límite cuando se llena** en vez de atender el cuello de botella.
5. **No medir.** Sin cycle time ni throughput, no sabes si mejoras.

### Para profundizar
- [The Kanban Guide](https://kanbanguides.org/english/)
- [Kanban overview — Azure Boards](https://learn.microsoft.com/en-us/azure/devops/boards/boards/kanban-overview?view=azure-devops)
- [What is Kanban? — Microsoft Learn](https://learn.microsoft.com/devops/plan/what-is-kanban)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Ley de Little: hay 12 tarjetas en progreso y el equipo termina 4 por semana. ¿Cycle time promedio?
> Cycle time = WIP / throughput = 12 / 4 = **3 semanas**. Bajar el WIP a 6 lo reduciría a ~1.5 semanas con el mismo ritmo.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuáles son las prácticas centrales de Kanban?
> Visualizar el flujo (tablero), limitar el trabajo en progreso (WIP), gestionar el flujo y mejorar con métricas (lead time, cycle time, throughput).

> [!question]- ¿Para qué sirve el límite de WIP?
> Evita empezar mucho y terminar poco: obliga a terminar antes de tomar lo siguiente y expone cuellos de botella.

> [!question]- ¿Cuándo Kanban encaja mejor que Scrum?
> Trabajo con llegada continua e impredecible: soporte, operación, mantenimiento.

