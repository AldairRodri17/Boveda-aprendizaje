---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Power BI
herramienta: Power BI Desktop
tags: [power-bi, laboratorio]
contenido: completo
---
# Lab 4 - Modelo estrella con tabla de calendario en Power BI

Parte de: [[17-Power-BI/03 Modelado de datos/_Índice Modelado de datos|Modelado de datos]]

**Cubre:** [[Modelo estrella (hechos y dimensiones)]] · [[Granularidad de una tabla de hechos]] · [[Relaciones (cardinalidad y dirección del filtro)]] · [[Tabla de calendario]] · [[Jerarquías]] · [[Relaciones activas e inactivas]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Convertir los datos del Lab 1 en un modelo estrella con tabla de calendario marcada, jerarquía de fechas y una relación inactiva para la fecha de envío.

## Pasos

1. Parte del archivo del Lab 1 (o recarga los CSV).
2. Crea la tabla Calendario (en Power Query o DAX) del 2024-01-01 al 2025-12-31 con Año, NumMes, Mes, Trimestre y AñoMes.
3. Márcala como tabla de fechas y ordena Mes por NumMes.
4. Relaciona Calendario[Fecha] → Ventas[Fecha] (activa) y Calendario[Fecha] → Ventas[FechaEnvio] (quedará inactiva).
5. Crea la jerarquía Año → Trimestre → Mes en Calendario.
6. Oculta las columnas clave de Ventas (IdProducto, Fecha, FechaEnvio) en la vista de informe.
7. Escribe en una nota (en esta carpeta) la granularidad de Ventas en una frase.
8. Construye una matriz con la jerarquía en filas y Ventas $; prueba el drill down.

## Criterios de terminado

- [ ] Calendario tiene 731 filas y está marcada como tabla de fechas.
- [ ] Hay exactamente 2 relaciones Calendario–Ventas, una punteada (inactiva).
- [ ] La matriz muestra 2024 = 750 y 2025 = 700, y los meses en orden enero, febrero...
- [ ] En la lista de campos de Ventas ya no aparecen las claves.
- [ ] La granularidad escrita es del tipo "una fila = una línea de venta de un producto en una fecha".

## Solución

> [!success]- Calendario en DAX
> ```dax
> Calendario =
> ADDCOLUMNS(
>     CALENDAR(DATE(2024, 1, 1), DATE(2025, 12, 31)),
>     "Año", YEAR([Date]),
>     "NumMes", MONTH([Date]),
>     "Mes", FORMAT([Date], "mmmm"),
>     "Trimestre", "T" & QUARTER([Date]),
>     "AñoMes", YEAR([Date]) * 100 + MONTH([Date]))
> ```
> Renombra `Date` a `Fecha`.

> [!success]- Resultados por mes
> 2024: ene 200, feb 550. 2025: ene 200, feb 500. Los demás meses vacíos.

> [!success]- Si al marcar la tabla de fechas da error
> La columna debe ser de tipo fecha, sin duplicados ni huecos. Revisa que no tenga hora.

## Notas personales

_Qué me costó, errores que cometí, dudas._
