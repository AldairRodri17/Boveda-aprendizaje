---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
contenido: completo
---
# Columnstore

Parte de: [[06-SQL-Server/04 Rendimiento/_Índice Rendimiento|Rendimiento]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un índice **columnstore** guarda los datos **por columnas** en lugar de por filas. En un índice tradicional (*rowstore*), cada página contiene filas completas; en un columnstore, los valores de una misma columna se guardan juntos y muy comprimidos. Está pensado para consultas **analíticas**: agregaciones y escaneos sobre millones de filas que leen pocas columnas ("ventas por mes y categoría de los últimos tres años"). En ese tipo de consultas puede ser varias veces más rápido y ocupar mucho menos espacio que un rowstore.

### Cómo funciona
1. Las filas se agrupan en **rowgroups** de hasta 1,048,576 filas (alrededor de un millón).
2. Dentro de cada rowgroup, cada columna se guarda como un **segmento** comprimido. Como los valores de una columna se parecen entre sí (muchas fechas, pocas categorías), la compresión es muy alta.
3. Cada segmento guarda metadatos de **mínimo y máximo**; si una consulta filtra `Fecha >= '2025-01-01'`, el motor salta los rowgroups cuyo máximo es anterior (*segment elimination*).
4. La consulta lee **solo las columnas que usa**: si la tabla tiene 40 columnas y la consulta usa 3, se lee una fracción de los datos.
5. Se ejecuta en **modo batch**: los operadores procesan bloques de alrededor de 900 filas a la vez en lugar de fila por fila, con mucho menos costo de CPU.

**El delta store**. Comprimir columnas es caro, así que las inserciones pequeñas no van directo al formato columnar. Van a un **delta store**, que es un rowgroup temporal en formato de filas (un árbol B normal). Cuando un delta store llega a 1,048,576 filas se **cierra**, y un proceso en segundo plano (*tuple mover*) lo comprime a formato columnar. Las cargas masivas de al menos 102,400 filas por lote se comprimen directamente, sin pasar por el delta store. Los `DELETE` marcan filas en un mapa de borrados, y un `UPDATE` es un borrado más una inserción; por eso el columnstore es malo para modificaciones frecuentes de filas individuales. `ALTER INDEX ... REORGANIZE` fuerza la compresión y limpia filas borradas.

```mermaid
flowchart LR
    A["INSERT pequeños"] --> B["Delta store (filas)"]
    B -->|"Llega a 1,048,576 filas"| C["Tuple mover"]
    D["Carga masiva de 102,400+ filas"] --> E["Rowgroup comprimido"]
    C --> E
    E --> F["Segmentos por columna con min y max"]
```

Hay dos variantes:
- **Clustered columnstore** (CCI): **es** la tabla; todos los datos están en formato columnar. Ideal para tablas de hechos de un data warehouse.
- **Nonclustered columnstore** (NCCI): un índice **adicional** sobre una tabla rowstore normal (OLTP). La tabla sigue sirviendo a la aplicación con sus índices de siempre, y el NCCI permite reportes analíticos sobre los mismos datos en tiempo real, sin un ETL a otra base. A esto Microsoft lo llama *análisis operativo en tiempo real*. El costo es que cada escritura de la aplicación también mantiene el NCCI.

### Ejemplo
```sql
-- Tabla de hechos para reportes (estilo data warehouse)
CREATE TABLE rep.VentasHecho (
    Fecha       date          NOT NULL,
    ProductoId  int           NOT NULL,
    ClienteId   int           NOT NULL,
    Cantidad    int           NOT NULL,
    Importe     decimal(12,2) NOT NULL
);
CREATE CLUSTERED COLUMNSTORE INDEX CCI_VentasHecho ON rep.VentasHecho;

-- Reporte típico: lee 3 columnas y salta rowgroups fuera del rango
SELECT DATEFROMPARTS(YEAR(Fecha), MONTH(Fecha), 1) AS Mes, SUM(Importe) AS Ventas
FROM   rep.VentasHecho
WHERE  Fecha >= '2025-01-01'
GROUP  BY DATEFROMPARTS(YEAR(Fecha), MONTH(Fecha), 1);

-- Análisis sobre la tabla OLTP sin cambiar la aplicación
CREATE NONCLUSTERED COLUMNSTORE INDEX NCCI_PedidoDetalle
    ON ventas.PedidoDetalle (PedidoId, ProductoId, Cantidad, PrecioUnitario);

-- Estado de los rowgroups (abiertos, cerrados, comprimidos)
SELECT OBJECT_NAME(object_id) AS Tabla, state_desc, total_rows, deleted_rows
FROM   sys.dm_db_column_store_row_group_physical_stats;
```

Para reportes pesados como los de Wee.Reporteria sobre años de movimientos, una tabla de hechos con CCI permite que la consulta de Dapper regrese en segundos lo que con rowstore tomaba minutos, sin cambiar el código C#.

### Cuándo usarlo y cuándo no
**Conviene**:
- Tablas grandes (millones de filas) consultadas con agregaciones y escaneos.
- Data warehouses, tablas de hechos, históricos que se cargan por lotes y casi no se actualizan.
- Reportes sobre una tabla OLTP (con NCCI), si el costo extra de escritura es aceptable.

**No conviene**:
- Tablas pequeñas: el beneficio no compensa.
- Cargas de trabajo con muchas búsquedas puntuales por clave (`WHERE PedidoId = @Id`): un rowstore con índice es mejor. (Puedes combinar un CCI con índices nonclustered rowstore para esas búsquedas.)
- Tablas con actualizaciones frecuentes de filas individuales.
- Cargas en lotes pequeños que dejan muchos delta stores abiertos y rowgroups comprimidos con pocas filas.

### Errores comunes
- **Insertar fila por fila** en un CCI: todo queda en delta stores y se pierde la compresión; carga por lotes grandes.
- **Usarlo para OLTP puro**: búsquedas puntuales y actualizaciones lentas.
- **No ordenar los datos al cargar**: si la columna de filtro (fecha) está mezclada en todos los rowgroups, no hay eliminación de segmentos.
- **Ignorar las filas borradas**: muchos `DELETE` inflan el mapa de borrados; reorganiza periódicamente.

### Para profundizar
- [Columnstore indexes: overview](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/columnstore-indexes-overview)
- [Columnstore indexes: design guidance](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/columnstore-indexes-design-guidance)
- [Get started with columnstore for real-time operational analytics](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un índice columnstore?
> Guarda los datos por **columna** en grupos de hasta ~1 millón de filas (rowgroups), muy comprimidos. Las consultas analíticas leen solo las columnas que usan y se ejecutan en *batch mode* (de a miles de filas).

> [!question]- ¿Cuándo conviene y cuándo no?
> Conviene en tablas grandes de hechos con agregaciones (`SUM`, `COUNT` sobre millones de filas). No conviene para búsquedas puntuales por clave ni tablas OLTP con muchas actualizaciones de filas sueltas.

> [!question]- ¿Qué es el *delta store*?
> Donde caen las inserciones pequeñas en formato de filas, hasta que se juntan suficientes y el *tuple mover* las comprime en un rowgroup. Las cargas masivas grandes (≥ 102,400 filas por lote) se comprimen directo.

> [!question]- ¿Qué es un columnstore nonclustered sobre una tabla OLTP?
> Un índice analítico adicional sobre una tabla de filas normal (HTAP): las transacciones usan los índices B-tree y los reportes el columnstore, sin copiar datos a otro sistema.

