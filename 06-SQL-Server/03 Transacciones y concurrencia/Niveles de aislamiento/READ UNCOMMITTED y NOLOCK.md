---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
contenido: completo
---
# READ UNCOMMITTED y NOLOCK

Parte de: [[Niveles de aislamiento]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`READ UNCOMMITTED` es el nivel de aislamiento más bajo: las lecturas **no piden bloqueos compartidos** y **no respetan** los bloqueos exclusivos de otras transacciones, así que pueden ver datos **no confirmados**. La pista de tabla `WITH (NOLOCK)` (sinónimo de `WITH (READUNCOMMITTED)`) aplica ese mismo comportamiento solo a una tabla de una consulta. Es muy popular porque "hace que las consultas no se bloqueen", pero el precio son datos incorrectos.

### Cómo funciona
Con `NOLOCK`, el lector:

- No toma bloqueos **S** sobre filas ni páginas.
- No espera a los **X** de los escritores.
- Sí toma un bloqueo de esquema **Sch-S** (así que igual espera a un DDL con Sch-M).
- Solo aplica a **lecturas**; en la tabla destino de un `UPDATE` o `DELETE` se ignora.

**Problemas que puede causar**:

1. **Lecturas sucias**: ves un cambio que luego se revierte. Un reporte muestra un pedido de $18,500 que nunca existió porque su transacción hizo `ROLLBACK`.
2. **Filas leídas dos veces o ninguna**: sin bloqueos, el motor puede recorrer la tabla en **orden de asignación** (por páginas en disco) en lugar del orden del índice. Si mientras lee ocurre un *page split* o una fila cambia de lugar (porque se actualizó su clave), puede volver a encontrar la fila más adelante (duplicada) o haberla pasado ya (perdida). Esto ocurre **aunque los datos estén confirmados**: un conteo con `NOLOCK` puede ser incorrecto sin que haya ninguna transacción revertida.
3. **Datos inconsistentes entre columnas o filas**: lees un pedido con su total nuevo y sus renglones viejos.
4. **Error 601** ("Could not continue scan with NOLOCK due to data movement"): la consulta falla porque los datos se movieron bajo el escaneo.

```mermaid
sequenceDiagram
  participant E as Escritor
  participant L as Lector con NOLOCK
  E->>E: BEGIN TRAN; UPDATE Precio = 99999
  L->>L: SELECT Precio WITH (NOLOCK): lee 99999
  E->>E: ROLLBACK (el precio vuelve a 1200)
  Note over L: El reporte ya mostró un valor que nunca existió
```

**La alternativa correcta**: si lo que quieres es que las lecturas **no se bloqueen** con los escritores, activa **RCSI** (`ALTER DATABASE ... SET READ_COMMITTED_SNAPSHOT ON`) o usa **SNAPSHOT** para transacciones de lectura. Con versiones de fila, el lector no espera y lee la **última versión confirmada**: obtienes la concurrencia que buscabas con `NOLOCK`, pero con datos correctos y consistentes por instrucción. Ver [[Snapshot y RCSI]].

### Ejemplo
```sql
-- Lo que se ve en muchos sistemas: NOLOCK en todas partes
SELECT p.PedidoId, SUM(d.Cantidad * d.PrecioUnitario) AS Total
FROM   ventas.Pedidos p WITH (NOLOCK)
JOIN   ventas.PedidoDetalle d WITH (NOLOCK) ON d.PedidoId = p.PedidoId
GROUP  BY p.PedidoId;

-- Alternativa: misma consulta, sin pistas, con RCSI activado en la base
ALTER DATABASE Tienda SET READ_COMMITTED_SNAPSHOT ON WITH ROLLBACK IMMEDIATE;

SELECT p.PedidoId, SUM(d.Cantidad * d.PrecioUnitario) AS Total
FROM   ventas.Pedidos p
JOIN   ventas.PedidoDetalle d ON d.PedidoId = p.PedidoId
GROUP  BY p.PedidoId;

-- Uso aceptable: diagnóstico aproximado del avance de una carga larga
SELECT COUNT_BIG(*) AS FilasAproximadas
FROM   stg.CargaMasiva WITH (NOLOCK);
```

Para "¿cuántas filas tiene una tabla enorme?" sin tocar los datos, también existe `sys.dm_db_partition_stats` (suma de `row_count`), que es metadato y no bloquea.

En la API, si encuentras `NOLOCK` en consultas `.sql` de Itzamna.Dionisio que alimentan decisiones (saldos, existencias, autorizaciones), quítalo y resuelve la contención con RCSI, índices o transacciones más cortas.

### Cuándo usarlo y cuándo no
- **Aceptable**: consultas de monitoreo o diagnóstico donde un número aproximado basta (avance de una carga, conteos para un tablero interno), o sobre tablas que nadie está modificando.
- **No lo uses** en nada que muestre importes, existencias, saldos, permisos o datos que el usuario tomará como ciertos, ni en datos que se van a reescribir (leer con `NOLOCK` y luego actualizar con base en eso).
- **No lo uses para "evitar deadlocks"**: los deadlocks entre escritores siguen ocurriendo.

### Errores comunes
1. **Creer que solo trae lecturas sucias**: también duplica o pierde filas confirmadas.
2. **Ponerlo por costumbre** en todas las consultas (o en una plantilla de SQL).
3. **Usarlo como solución de rendimiento**: no reduce lecturas ni CPU, solo evita esperas; si la consulta es lenta, falta un índice.
4. **No conocer RCSI**, que resuelve el bloqueo lector–escritor sin sacrificar corrección.

### Para profundizar
- [Sugerencias de tabla (Transact-SQL): NOLOCK y READUNCOMMITTED](https://learn.microsoft.com/sql/t-sql/queries/hints-transact-sql-table)
- [SET TRANSACTION ISOLATION LEVEL](https://learn.microsoft.com/sql/t-sql/statements/set-transaction-isolation-level-transact-sql)
- [Guía de bloqueo y control de versiones de filas](https://learn.microsoft.com/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace `WITH (NOLOCK)`?
> Lee la tabla en `READ UNCOMMITTED`: no pide bloqueos compartidos ni respeta los exclusivos de otros. Puede leer datos que luego se revierten.

> [!question]- Además de lecturas sucias, ¿qué otros problemas puede dar `NOLOCK`?
> Leer filas **dos veces** o **saltárselas** cuando hay *page splits* durante el recorrido, e incluso el error 601 (datos movidos). No es un «acelerador seguro».

> [!question]- ¿Qué alternativa a `NOLOCK` evita que las lecturas se bloqueen sin leer datos sucios?
> Activar RCSI (`READ_COMMITTED_SNAPSHOT ON`) o usar `SNAPSHOT`: los lectores ven la última versión confirmada sin esperar a los escritores.

## Práctica

- [[Lab 6 - Bloqueos, niveles de aislamiento y deadlock en dos sesiones]]

