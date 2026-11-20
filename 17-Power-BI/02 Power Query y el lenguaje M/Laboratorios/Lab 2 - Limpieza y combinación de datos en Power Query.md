---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Power BI
herramienta: Power BI Desktop (Power Query)
tags: [power-bi, laboratorio]
---
# Lab 2 - Limpieza y combinación de datos en Power Query

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

**Cubre:** [[Power Query - interfaz y pasos aplicados]] · [[Power Query - tipos de datos]] · [[Power Query - limpieza de datos]] · [[Power Query - columnas condicionales y personalizadas]] · [[Power Query - combinar consultas (merge y append)]] · [[Power Query - agrupar, pivotar y despivotar]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Limpiar un archivo "sucio", anexar dos años de ventas, combinarlas con el catálogo y despivotar un presupuesto, viendo el código M de cada paso.

## Pasos

1. Crea en esta carpeta `ventas2024.csv` con las ventas 1–3 y `ventas2025.csv` con las 4–5 del Lab 1, pero agrega a `ventas2024.csv` dos filas de título arriba ("Reporte de ventas", vacía) y escribe un cliente con espacios: agrega columna `Cliente` con valores `  ana LÓPEZ `, `Luis Pérez`, `luis pérez `.
2. Carga ambos archivos. En `ventas2024` quita las 2 filas superiores, promueve encabezados y asigna tipos.
3. Normaliza `Cliente` con Recortar y *Poner en mayúsculas cada palabra*.
4. Anexa ambas consultas en una nueva `Ventas` y desactiva la carga de las dos originales.
5. Combina `Ventas` con `Productos` (izquierda externa) y expande solo `Categoria`.
6. Agrega la columna personalizada `Importe` y la condicional `Rango` (Alto ≥ 400, Medio ≥ 200, Bajo).
7. Crea `presupuesto.csv` con `Categoria,Ene,Feb` (Bebidas 300,300; Equipo 400,400; Alimentos 100,100), cárgalo y despivota los meses.
8. Abre el *Editor avanzado* de `Ventas` y lee el código M generado.
9. Haz un merge anti izquierda de Ventas contra Productos para comprobar que no hay productos huérfanos.

## Criterios de terminado

- [ ] `Ventas` tiene 5 filas y columnas tipadas (Fecha = fecha, Cantidad = entero, PrecioUnitario = decimal).
- [ ] `Cliente` muestra exactamente `Ana López`, `Luis Pérez` (sin espacios extra).
- [ ] `Rango` da Alto solo para la venta 5 (500); Medio para 200, 250, 300, 200; ninguno Bajo.
- [ ] `Presupuesto` despivotado tiene 6 filas con columnas Categoria, Mes, Monto.
- [ ] El merge anti izquierda devuelve 0 filas.
- [ ] Solo `Ventas`, `Productos` y `Presupuesto` se cargan al modelo.

## Solución

> [!success]- Código M de referencia para ventas2024
> ```m
> let
>     Origen = Csv.Document(File.Contents(RutaCarpeta & "ventas2024.csv"), [Delimiter = ",", Encoding = 65001]),
>     SinTop = Table.Skip(Origen, 2),
>     Encab = Table.PromoteHeaders(SinTop, [PromoteAllScalars = true]),
>     Tipos = Table.TransformColumnTypes(Encab, {{"Fecha", type date}, {"Cantidad", Int64.Type}, {"PrecioUnitario", type number}}),
>     Cliente = Table.TransformColumns(Tipos, {{"Cliente", each Text.Proper(Text.Trim(_)), type text}})
> in
>     Cliente
> ```
> (`RutaCarpeta` es un parámetro de texto que termina en `\`.)

> [!success]- Anexar, combinar y columnas
> ```m
> let
>     Todas = Table.Combine({ventas2024, ventas2025}),
>     M = Table.NestedJoin(Todas, {"IdProducto"}, Productos, {"IdProducto"}, "P", JoinKind.LeftOuter),
>     X = Table.ExpandTableColumn(M, "P", {"Categoria"}),
>     Imp = Table.AddColumn(X, "Importe", each [Cantidad] * [PrecioUnitario], type number),
>     Rango = Table.AddColumn(Imp, "Rango", each if [Importe] >= 400 then "Alto" else if [Importe] >= 200 then "Medio" else "Bajo", type text)
> in
>     Rango
> ```

> [!success]- Despivotar
> ```m
> Table.UnpivotOtherColumns(PresupuestoTipado, {"Categoria"}, "Mes", "Monto")
> ```

> [!success]- Si `Luis Pérez` aparece dos veces distinto
> Revisa que el paso de Trim vaya antes de Proper y que no haya espacios no separables (usa también `Text.Clean`).

## Notas personales

_Qué me costó, errores que cometí, dudas._
