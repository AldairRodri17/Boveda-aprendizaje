---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Power BI
herramienta: Power BI Desktop + DAX Studio (gratuito)
tags: [power-bi, laboratorio]
---
# Lab 7 - Consultas DAX con la vista de consultas y DAX Studio

Parte de: [[17-Power-BI/05 Ayudas para crear DAX más rápido/_Índice Ayudas para crear DAX más rápido|Ayudas para crear DAX más rápido]]

**Cubre:** [[Vista de consultas DAX (DAX query view)]] · [[DAX Studio]] · [[Consultas DAX (EVALUATE, DEFINE y MEASURE)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Probar medidas como consultas antes de agregarlas al modelo y medir su rendimiento.

## Pasos

1. Abre el archivo del Lab 6 y entra a la vista de consultas DAX.
2. Escribe una consulta EVALUATE que devuelva ventas por categoría y año.
3. Define con DEFINE MEASURE una medida temporal `Ticket` y agrégala a la consulta.
4. Usa *Actualizar modelo con cambios* para guardar `Ticket` en el modelo.
5. Instala DAX Studio, conéctalo al modelo abierto de Desktop (*PBI / SSDT Model*).
6. Ejecuta la misma consulta con *Server Timings* activado y anota tiempo total y % SE/FE.
7. Abre *Advanced → View Metrics* y anota la columna más pesada.
8. (Opcional) Si tienes acceso a Copilot, pídele que explique tu consulta y compara su explicación con la tuya.

## Criterios de terminado

- [ ] La consulta devuelve 4 filas: Bebidas-2024 500, Equipo-2024 250, Equipo-2025 500, Alimentos-2025 200.
- [ ] La medida `Ticket` aparece en el modelo tras actualizar.
- [ ] Tienes anotados los tiempos de Server Timings.
- [ ] Identificaste la columna con mayor tamaño en View Metrics.

## Solución

> [!success]- Consulta de referencia
> ```dax
> DEFINE
>     MEASURE _Medidas[Ticket] = DIVIDE([Ventas $], COUNTROWS(Ventas))
> EVALUATE
> SUMMARIZECOLUMNS(
>     Productos[Categoria], Calendario[Año],
>     "Ventas", [Ventas $],
>     "Ticket", [Ticket])
> ORDER BY Productos[Categoria], Calendario[Año]
> ```

> [!success]- Lectura de Server Timings
> Con datos tan pequeños los tiempos serán de pocos milisegundos; lo importante es ubicar dónde se ve SE vs FE y las consultas xmSQL generadas.

## Notas personales

_Qué me costó, errores que cometí, dudas._
