---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# DAX - VALUES y DISTINCT

Parte de: [[17-Power-BI/04 DAX/_Índice DAX|DAX]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`VALUES` y `DISTINCT` devuelven una **tabla de una columna con los valores únicos visibles** en el contexto de filtro actual. Con una tabla completa, `VALUES(Tabla)` devuelve sus filas visibles. Son la base de muchas tablas virtuales, de los conteos de distintos y de los títulos dinámicos.

### Cómo funciona
- `VALUES(Productos[Categoria])` en la celda "2025" devuelve {Equipo, Alimentos}: solo las categorías que sobreviven a los filtros. Ojo: en un modelo con relación Productos → Ventas, filtrar Calendario **no** filtra Productos (el filtro va de las dimensiones a los hechos). Para obtener "categorías con ventas" debes contar sobre Ventas o filtrar con la medida.
- **Diferencia con DISTINCT**: cuando hay filas en la tabla de hechos cuya clave no existe en la dimensión (integridad referencial rota), el motor agrega una **fila en blanco** "virtual" a la dimensión. `VALUES` la incluye; `DISTINCT` no. Con `VALUES(Tabla)` también se respeta esa fila; `DISTINCT(Tabla)` la omite.
- Si el resultado tiene exactamente una fila y una columna, DAX lo convierte automáticamente en escalar. Por eso `VALUES(col)` sirve como "el valor seleccionado"... solo si hay uno. Si hay varios, da el error de "tabla con varios valores donde se esperaba un único valor". De ahí el patrón `IF(HASONEVALUE(col), VALUES(col))` y su atajo `SELECTEDVALUE` ([[DAX - SELECTEDVALUE, ISINSCOPE y HASONEVALUE]]).

### Ejemplo
> [!example] Modelo de ejemplo de esta sección
> - `Productos`: P1 Café (Bebidas, Negro), P2 Molino (Equipo, Rojo), P3 Galletas (Alimentos, Café).
> - `Ventas` (Cantidad × PrecioUnitario, CostoUnitario): 1) 2024-01-15 P1 2×100 (60); 2) 2024-02-10 P2 1×250 (150); 3) 2024-02-20 P1 3×100 (60); 4) 2025-01-05 P3 5×40 (25); 5) 2025-02-14 P2 2×250 (150).
> - `Calendario` (Fecha, Año, NumMes, Mes, AñoMes, Trimestre) relacionado 1:* con `Ventas[Fecha]`; `Productos` 1:* con `Ventas[IdProducto]`.
> - Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total **1 450**; Bebidas 500, Equipo 750, Alimentos 200; 2024 = 750, 2025 = 700.

```dax
-- Categorías con ventas en el contexto (usando la medida como condición)
Categorias con venta =
COUNTROWS(FILTER(VALUES(Productos[Categoria]), [Ventas $] > 0))
-- 2025: Equipo (500) y Alimentos (200) → 2. Total: 3.

-- Alternativa sobre la tabla de hechos: categoría por la relación
Categorias con venta 2 =
COUNTROWS(SUMMARIZE(Ventas, Productos[Categoria]))  -- también 2 en 2025

-- Lista de productos visibles
Lista productos =
CONCATENATEX(VALUES(Productos[Producto]), Productos[Producto], ", ", Productos[Producto], ASC)
-- Sin filtros: "Café, Galletas, Molino"

-- Categoría seleccionada o "Varias"
Categoria elegida =
IF(HASONEVALUE(Productos[Categoria]), VALUES(Productos[Categoria]), "Varias")
```

Para la lista de productos *con ventas* en 2025, filtra con la medida: `CONCATENATEX(FILTER(VALUES(Productos[Producto]), [Ventas $] > 0), Productos[Producto], ", ")` → "Galletas, Molino" (o en el orden que indiques).

### Cuándo usarlo y cuándo no
- **VALUES** es la opción habitual: incluye la fila en blanco, lo que te ayuda a *ver* problemas de integridad (ventas sin producto).
- **DISTINCT** cuando quieres explícitamente excluir esa fila virtual, por ejemplo al construir una lista para mostrar.
- Para un solo valor seleccionado, usa **SELECTEDVALUE**; es más corto y seguro.
- Para iterar "por mes" o "por cliente", `VALUES(Dim[Clave])` es el patrón base de tablas virtuales ([[DAX - tablas virtuales]]).

### Errores comunes
- **Usar VALUES como escalar sin verificar**: funciona en la fila de detalle y truena en el total ("varios valores").
- **Esperar que VALUES de una dimensión solo traiga valores con ventas**: devuelve los valores visibles de la dimensión, tengan o no hechos. Filtra con la medida o resume la tabla de hechos.
- **Contar categorías con `COUNTROWS(VALUES(Ventas[IdProducto]))` y pensar que son categorías**: estás contando productos.
- **Ignorar la fila en blanco**: aparece como "(En blanco)" en visuales; corrige la integridad en el origen.

### Para profundizar
- [Función VALUES](https://learn.microsoft.com/en-us/dax/values-function-dax)
- [Función DISTINCT (columna)](https://learn.microsoft.com/en-us/dax/distinct-function-dax)
- [VALUES en DAX Guide](https://dax.guide/values/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Datos de ejemplo (para los ejercicios con resultado numérico): Productos P1 Café (Bebidas), P2 Molino (Equipo), P3 Galletas (Alimentos). Ventas: 1) 2024-01-15 P1 2×100; 2) 2024-02-10 P2 1×250; 3) 2024-02-20 P1 3×100; 4) 2025-01-05 P3 5×40; 5) 2025-02-14 P2 2×250. Medida base: `Ventas $ = SUMX(Ventas, Ventas[Cantidad] * Ventas[PrecioUnitario])` → total 1 450. Medida que cuente las categorías con ventas en el contexto actual. ¿Cuánto da para 2025?
> ```dax
> Categorías con venta = COUNTROWS(SUMMARIZE(Ventas, Productos[Categoria]))
> ```
> 2025: Alimentos y Equipo → 2. (Con `COUNTROWS(VALUES(Productos[Categoria]))` darías 3, porque filtrar Calendario no filtra Productos.)

> [!question]- Medida que liste los productos visibles separados por coma.
> ```dax
> Lista productos = CONCATENATEX(VALUES(Productos[Producto]), Productos[Producto], ", ")
> ```

> [!question]- Medida que muestre la categoría seleccionada o "Varias" si hay más de una, usando HASONEVALUE y VALUES.
> ```dax
> Categoría = IF(HASONEVALUE(Productos[Categoria]), VALUES(Productos[Categoria]), "Varias")
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué devuelve VALUES(columna)?
> La lista de valores distintos visibles en el contexto actual, incluyendo la fila en blanco si hay claves huérfanas en la relación.

> [!question]- Diferencia entre VALUES y DISTINCT.
> DISTINCT no incluye la fila en blanco agregada por relaciones inválidas; VALUES sí. Con datos limpios dan lo mismo.

> [!question]- ¿Para qué se usa `IF(HASONEVALUE(col), VALUES(col))`?
> Para obtener el valor cuando hay exactamente uno seleccionado; hoy se escribe más simple con `SELECTEDVALUE(col)`.

