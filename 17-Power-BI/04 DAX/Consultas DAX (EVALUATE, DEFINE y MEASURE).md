---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
contenido: completo
---
# Consultas DAX (EVALUATE, DEFINE y MEASURE)

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Además de medidas, DAX es un **lenguaje de consulta**: una consulta DAX pide al modelo una tabla de resultados, igual que un `SELECT` en SQL Server. Es lo que ejecutan por dentro los visuales y lo que escribes en la [[Vista de consultas DAX (DAX query view)]], en [[DAX Studio]], en informes paginados o desde código (.NET con ADOMD.NET o la API REST *executeQueries*).

### Cómo funciona
Estructura de una consulta:

```dax
DEFINE                                   -- opcional: definiciones locales
    VAR Anio = 2025
    MEASURE Ventas[Ticket] = DIVIDE([Ventas $], COUNTROWS(Ventas))
    COLUMN Productos[Etiqueta] = Productos[Producto] & " (" & Productos[Categoria] & ")"
    TABLE Top2 = TOPN(2, ALL(Productos[Categoria]), [Ventas $])
EVALUATE                                 -- obligatorio: expresión de TABLA
    <expresión de tabla>
ORDER BY <columna> [ASC | DESC], ...     -- opcional
START AT <valor>, ...                    -- opcional, requiere ORDER BY
```

- **EVALUATE** ejecuta una expresión que devuelve una tabla y regresa sus filas. Puede haber varios EVALUATE en la misma consulta; cada uno devuelve un resultado.
- **DEFINE** declara objetos que solo existen durante la consulta: `MEASURE` (medidas temporales, ideales para probar antes de agregarlas al modelo), `VAR`, `COLUMN`, `TABLE` y también funciones definidas por el usuario. Una medida definida con el mismo nombre que una del modelo **la sustituye** dentro de la consulta: así pruebas una corrección sin tocar el modelo.
- **ORDER BY** ordena el resultado; **START AT** indica desde qué valor de las columnas de ORDER BY empezar a devolver filas (útil para paginar).
- Para devolver un **escalar** se envuelve en llaves, que crean una tabla de una fila: `EVALUATE { [Ventas $] }`. También existe `ROW("Nombre", expresión)`.
- Las funciones de tabla habituales en consultas son `SUMMARIZECOLUMNS`, `ADDCOLUMNS`, `FILTER`, `TOPN`, `CALCULATETABLE` y `SELECTCOLUMNS` ([[DAX - SUMMARIZE, SUMMARIZECOLUMNS y ADDCOLUMNS]]).

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

```dax
-- 1) Tabla completa
EVALUATE Productos

-- 2) Medida temporal por categoría, de mayor a menor
DEFINE
    MEASURE Ventas[Ticket] = DIVIDE([Ventas $], COUNTROWS(Ventas))
EVALUATE
SUMMARIZECOLUMNS(
    Productos[Categoria],
    "Ventas", [Ventas $],
    "Ticket", [Ticket]
)
ORDER BY [Ventas] DESC
-- Equipo 750 / 375; Bebidas 500 / 250; Alimentos 200 / 200

-- 3) Un solo valor
EVALUATE { [Ventas $] }        -- 1 450

-- 4) Filtro y paginación
EVALUATE
CALCULATETABLE(
    SUMMARIZECOLUMNS(Calendario[AñoMes], "Ventas", [Ventas $]),
    Calendario[Año] = 2024
)
ORDER BY Calendario[AñoMes]
START AT "2024-02"             -- solo feb-24 (550), si AñoMes es texto "AAAA-MM"
```

Desde el stack .NET puedes ejecutar la misma consulta contra un modelo publicado: con la API REST de Power BI (`POST .../datasets/{id}/executeQueries`, cuerpo JSON con `"query": "EVALUATE ..."`) o con ADOMD.NET a través del punto de conexión XMLA (requiere Premium por usuario o una capacidad Premium/Fabric). Es útil, por ejemplo, para que Wee.Reporteria genere un Excel con exactamente los mismos números que el informe.

### Cuándo usarlo y cuándo no
- **Probar y depurar medidas** con `DEFINE MEASURE` antes de guardarlas en el modelo.
- **Ver tablas virtuales** que dentro de una medida no puedes inspeccionar.
- **Analizar rendimiento**: copia la consulta de un visual desde Performance Analyzer y ejecútala en DAX Studio.
- **Extraer datos** del modelo a otras herramientas o documentar resultados esperados en pruebas.
- No reemplaza a SQL para extracciones masivas desde la base: si los datos están en SQL Server, consúltalos allí.

### Errores comunes
- **EVALUATE con una expresión escalar** (`EVALUATE [Ventas $]`): debe ser tabla; usa llaves o ROW.
- **ORDER BY con un nombre de columna inexistente** en el resultado (por ejemplo, ordenar por `[Ventas $]` cuando la columna se llama "Ventas").
- **Creer que DEFINE MEASURE guarda la medida**: es temporal; en la vista de consultas DAX hay que usar *Actualizar modelo* para agregarla.
- **START AT sin ORDER BY**, o con más valores que columnas de ORDER BY.

### Para profundizar
- [Consultas DAX (Microsoft Learn)](https://learn.microsoft.com/en-us/dax/dax-queries)
- [Instrucción EVALUATE](https://learn.microsoft.com/en-us/dax/evaluate-statement-dax)
- [EVALUATE en DAX Guide](https://dax.guide/st/evaluate/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Datos de ejemplo (para los ejercicios con resultado numérico): Productos P1 Café (Bebidas), P2 Molino (Equipo), P3 Galletas (Alimentos). Ventas: 1) 2024-01-15 P1 2×100; 2) 2024-02-10 P2 1×250; 3) 2024-02-20 P1 3×100; 4) 2025-01-05 P3 5×40; 5) 2025-02-14 P2 2×250. Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total 1 450. Consulta que devuelva la tabla Productos completa.
> ```dax
> EVALUATE Productos
> ```

> [!question]- Consulta con una medida temporal `Ticket` por categoría, ordenada de mayor a menor.
> ```dax
> DEFINE
>     MEASURE Ventas[Ticket] = DIVIDE([Ventas $], COUNTROWS(Ventas))
> EVALUATE
> SUMMARIZECOLUMNS(Productos[Categoria], "Ventas", [Ventas $], "Ticket", [Ticket])
> ORDER BY [Ventas] DESC
> ```
> Equipo 750 / 2 = 375; Bebidas 500 / 2 = 250; Alimentos 200 / 1 = 200.

> [!question]- Consulta que devuelva un solo valor (las ventas totales).
> ```dax
> EVALUATE { [Ventas $] }
> ```
> → 1 450. Las llaves convierten un escalar en tabla de una fila.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace EVALUATE?
> Ejecuta una expresión de tabla y devuelve sus filas. Toda consulta DAX necesita al menos un EVALUATE.

> [!question]- ¿Para qué sirve DEFINE MEASURE?
> Definir medidas temporales (solo para la consulta) para probarlas antes de agregarlas al modelo.

> [!question]- ¿Qué hace ORDER BY y START AT?
> Ordenan el resultado (y START AT indica desde qué valor empezar en ese orden).

## Práctica

- [[Lab 7 - Consultas DAX con la vista de consultas y DAX Studio]]

