---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Power BI
herramienta: Power BI Desktop
tags: [power-bi, laboratorio]
---
# Lab 6 - Medidas con CALCULATE y comparativos de tiempo en Power BI

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

**Cubre:** [[DAX - CALCULATE]] · [[DAX - FILTER, ALL, ALLEXCEPT y REMOVEFILTERS]] · [[DAX - variables (VAR y RETURN)]] · [[DAX - IF, SWITCH y DIVIDE]] · [[DAX - inteligencia de tiempo]] · [[DAX - YTD, mismo periodo del año anterior y comparativos]] · [[DAX - totales acumulados]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Escribir una tabla de medidas con % del total, YTD, año anterior, variación y acumulado, y validar cada resultado contra los datos de ejemplo.

## Pasos

1. Sobre el modelo del Lab 4, crea una tabla vacía `_Medidas` para agrupar medidas.
2. Crea `% del total` (ventas de la fila / ventas de todos los productos).
3. Crea `Ventas YTD`, `Ventas AA` (mismo periodo año anterior), `Var vs AA %` (con VAR y DIVIDE) y `Ventas acumuladas`.
4. Crea `Semáforo` que devuelva "Sube", "Baja" o blank según Var vs AA %.
5. Arma una matriz con Año y Mes en filas y todas las medidas en valores.
6. Arma otra matriz con Categoria en filas y Ventas $ y % del total.
7. Usa *Ver como* o filtros para comprobar que % del total sigue sumando 100 % aunque filtres un año.

## Criterios de terminado

- [ ] % del total: Equipo 51.72 %, Bebidas 34.48 %, Alimentos 13.79 %.
- [ ] Feb-2025: Ventas YTD 700, Ventas AA 550, Var vs AA % −9.09 %.
- [ ] Ventas acumuladas: 200, 750, 950, 1 450 en los cuatro meses con venta.
- [ ] Ninguna medida muestra error ni infinito en meses sin datos.
- [ ] Todas las medidas usan columnas de Calendario (no Ventas[Fecha]) en la inteligencia de tiempo.

## Solución

> [!success]- Medidas
> ```dax
> % del total = DIVIDE([Ventas $], CALCULATE([Ventas $], REMOVEFILTERS(Productos)))
> Ventas YTD = TOTALYTD([Ventas $], Calendario[Fecha])
> Ventas AA = CALCULATE([Ventas $], SAMEPERIODLASTYEAR(Calendario[Fecha]))
> Var vs AA % =
> VAR A = [Ventas $]
> VAR B = [Ventas AA]
> RETURN DIVIDE(A - B, B)
> Ventas acumuladas =
> VAR F = MAX(Calendario[Fecha])
> RETURN CALCULATE([Ventas $], Calendario[Fecha] <= F, REMOVEFILTERS(Calendario))
> Semáforo =
> VAR V = [Var vs AA %]
> RETURN SWITCH(TRUE(), ISBLANK(V), BLANK(), V >= 0, "Sube", "Baja")
> ```

> [!success]- Valores esperados por mes
> | Mes | Ventas | YTD | AA | Var | Acum |
> |---|---|---|---|---|---|
> | 2024-01 | 200 | 200 | | | 200 |
> | 2024-02 | 550 | 750 | | | 750 |
> | 2025-01 | 200 | 200 | 200 | 0 % | 950 |
> | 2025-02 | 500 | 700 | 550 | −9.09 % | 1 450 |

> [!success]- Si % del total da 100 % en todas las filas
> Quitaste filtros de la tabla equivocada o usaste VALUES. Debe quitar el filtro de Productos (la tabla de las filas).

## Notas personales

_Qué me costó, errores que cometí, dudas._
