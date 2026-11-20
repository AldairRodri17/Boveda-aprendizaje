---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Automatización, DevOps y operación
herramienta: Papel, hoja de cálculo o Python; plantilla de postmortem en esta bóveda
tags: [automatizacion-devops, laboratorio]
---
# Lab 3 - Simulacro de incidente con SLO, error budget y postmortem

Parte de: [[08-Automatizacion-DevOps/02 Operación en producción/_Índice Operación en producción|Operación en producción]]

**Cubre:** [[SLI, SLO y SLA]] · [[Error budgets]] · [[Manejo de incidentes]] · [[Postmortems sin culpables]] · [[Runbooks]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Practicar con números reales cómo un incidente consume el presupuesto de error y redactar un postmortem sin culpables con acciones concretas.

## Escenario

API **Pedidos**. SLI: % de requests con código < 500. **SLO: 99.9 % en 30 días.** Tráfico del mes: **3 000 000** requests. Antes del incidente ya llevabas **600** requests fallidos este mes. Tráfico durante el incidente: **1 000 requests/min**.

Bitácora (día 12):
- 14:02 El pipeline hace swap de `staging` a producción con la versión 1.8.0.
- 14:05 La tasa de error sube a **5 %**: los pedidos con cupón lanzan `NullReferenceException`.
- 14:20 Un cliente reporta el problema por correo.
- 14:24 La persona de guardia reconoce el aviso. Ninguna alerta sonó: el umbral era «errores > 10 % durante 5 min».
- 14:31 En Application Insights se correlaciona el aumento con el despliegue.
- 14:37 Se hace swap de regreso (rollback).
- 14:40 La tasa de error vuelve a lo normal.

## Pasos

1. Calcula el presupuesto de error del mes en requests.
2. Calcula cuántos requests fallaron durante el incidente (14:05 a 14:40) y qué porcentaje del presupuesto consumió.
3. Calcula el presupuesto restante sumando los 600 fallos previos.
4. Calcula la *burn rate* durante el incidente.
5. Calcula el tiempo de detección (inicio → reconocimiento) y el de mitigación (reconocimiento → normalidad).
6. Decide, según una política de presupuesto que tú definas, si se congelan los despliegues de funcionalidades.
7. Redacta el postmortem en una nota nueva: resumen, impacto, línea de tiempo, detección, causa raíz y factores contribuyentes, qué salió bien, acciones (dueño y fecha).
8. Escribe un runbook corto «Rollback de API por swap de slot» que la guardia pudo haber seguido.

## Criterios de terminado

- [ ] Tus números coinciden con la solución (presupuesto, fallos, % consumido, restante, burn rate, tiempos).
- [ ] El postmortem no menciona culpables y ninguna causa es «error humano».
- [ ] Hay al menos 3 acciones concretas y verificables, cada una con dueño y fecha.
- [ ] Una de las acciones ataca la **detección** (la alerta no sonó).
- [ ] El runbook tiene pasos, comando o clic exacto, verificación y a quién escalar.

## Solución

> [!success]- Solución: números
> - Presupuesto: 3 000 000 × 0.001 = **3 000** requests.
> - Fallos del incidente: 35 min × 1 000 × 0.05 = **1 750** → 1 750 / 3 000 ≈ **58.3 %** del presupuesto.
> - Total del mes: 600 + 1 750 = 2 350 → **78.3 %** consumido; quedan **650**.
> - Burn rate: 0.05 / 0.001 = **50** (a ese ritmo, el presupuesto de 30 días se acaba en 30/50 = 0.6 días ≈ 14.4 h).
> - Detección: 14:05 → 14:24 = **19 min**. Mitigación: 14:24 → 14:40 = **16 min**.
> - Política de ejemplo: si se consume > 75 %, solo se despliegan arreglos de confiabilidad hasta el siguiente periodo → **se congela**.

> [!success]- Solución: causas y acciones de ejemplo
> **Causa raíz**: el código nuevo de descuentos no manejaba pedidos con cupón sin fecha de vigencia, y ninguna prueba cubría ese caso.
>
> **Factores contribuyentes**: la alerta por umbral fijo (10 %) no detecta errores del 5 %; staging no tenía datos con cupones; el swap mandó el 100 % del tráfico de golpe; nadie revisó métricas tras el despliegue.
>
> **Acciones**:
> 1. Prueba unitaria y de integración para cupones sin vigencia — dueño: equipo Pedidos — fecha.
> 2. Alerta por **burn rate** (p. ej. > 14.4 en 1 h, que equivale a gastar 2 % del presupuesto mensual en una hora) con enlace al runbook — dueño: guardia — fecha.
> 3. Canary: enrutar 10 % del tráfico al slot nuevo 15 min antes del swap completo — dueño: DevOps — fecha.
> 4. Datos de prueba con cupones en staging — fecha.
>
> **Qué salió bien**: el rollback por swap tardó 3 minutos.

> [!success]- Solución: runbook de ejemplo
> 1. Confirma en Application Insights que el aumento de errores empieza después del último despliegue (marcador de release o hora del swap).
> 2. Portal → App Service → *Deployment slots* → **Swap** (`staging` ↔ `production`), o `az webapp deployment slot swap -g <rg> -n <app> --slot staging --target-slot production`.
> 3. Verifica `/health` en 200 y la tasa de error de los últimos 5 min en el dashboard.
> 4. Avisa en el canal del incidente con la hora de mitigación.
> 5. Si en 10 min no mejora, escala al líder técnico.

## Notas personales

_Qué me costó, errores que cometí, dudas._
