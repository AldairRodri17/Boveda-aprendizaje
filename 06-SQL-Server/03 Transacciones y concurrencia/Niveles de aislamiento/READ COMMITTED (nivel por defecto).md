---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# READ COMMITTED (nivel por defecto)

Parte de: [[Niveles de aislamiento]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`READ COMMITTED` es el nivel de aislamiento **predeterminado** de SQL Server. Garantiza que **solo lees datos confirmados**: nunca verás un cambio que otra transacción todavía podría revertir (no hay lecturas sucias). No garantiza que una fila mantenga su valor si la vuelves a leer más tarde en la misma transacción. Tiene dos implementaciones: **con bloqueos** (la clásica de SQL Server) y **con versiones de fila** (RCSI, la predeterminada en Azure SQL Database).

### Cómo funciona
**READ COMMITTED con bloqueos** (RCSI apagado):

1. Para leer una fila, la sesión pide un bloqueo **S**.
2. Si otra transacción tiene un **X** sobre esa fila (la está modificando sin confirmar), el lector **espera** hasta que confirme o revierta.
3. Al terminar de leer la fila, el S se **libera de inmediato**; no se mantiene hasta el final de la transacción.

Consecuencias: los lectores esperan a los escritores (y los escritores a los lectores mientras leen). Como el S se suelta enseguida, otra transacción puede modificar la fila justo después, y si la lees de nuevo verás otro valor (**lectura no repetible**), o una consulta repetida puede traer filas nuevas (**fantasmas**). Incluso dentro de **una sola** instrucción que recorre mucha tabla, una fila puede leerse dos veces o ninguna si otra transacción la mueve de lugar en el índice mientras el escaneo avanza.

**READ COMMITTED SNAPSHOT (RCSI)** (`READ_COMMITTED_SNAPSHOT ON`):

1. Cuando alguien modifica una fila, la versión anterior se guarda en el *version store*.
2. Un lector **no pide bloqueos S**: si la fila está siendo modificada, lee la **última versión confirmada al inicio de la instrucción**.
3. Resultado: los lectores **no bloquean ni son bloqueados** por los escritores, y cada instrucción ve una foto consistente de los datos. Los escritores siguen bloqueándose entre sí.

Mismo nivel, mismas garantías formales (sin lecturas sucias, con no repetibles y fantasmas entre instrucciones), pero con mucha menos espera. El costo y la activación están en [[Snapshot y RCSI]].

```mermaid
sequenceDiagram
  participant E as Escritor
  participant L as Lector
  E->>E: BEGIN TRAN; UPDATE Stock = 4 (X)
  alt RC con bloqueos
    L->>L: SELECT Stock: espera el X
    E->>E: COMMIT
    L->>L: lee 4
  else RCSI
    L->>L: SELECT Stock: lee versión confirmada (5) sin esperar
  end
```

### Ejemplo
**El problema de "leer, calcular y escribir"**. En READ COMMITTED (con o sin RCSI):

```sql
-- Sesión A y Sesión B ejecutan esto casi al mismo tiempo para el mismo producto
DECLARE @Stock int;
SELECT @Stock = Piezas FROM inventario.Existencias WHERE ProductoId = 1;  -- ambas leen 5
-- ... la API valida que haya existencias ...
UPDATE inventario.Existencias SET Piezas = @Stock - 1 WHERE ProductoId = 1; -- ambas escriben 4
```

Ambas leen 5 porque el bloqueo S (si lo hay) se libera tras leer; ambas escriben 4. Se vendieron dos piezas y el inventario solo bajó una: **actualización perdida**. Soluciones:

```sql
-- 1) Una sola instrucción atómica que lee y escribe (la mejor)
UPDATE inventario.Existencias
SET    Piezas = Piezas - @Cantidad
WHERE  ProductoId = @ProductoId AND Piezas >= @Cantidad;
IF @@ROWCOUNT = 0 THROW 50010, N'Sin existencias suficientes.', 1;

-- 2) Si necesitas leer primero, bloquea para actualizar desde la lectura
BEGIN TRAN;
SELECT @Stock = Piezas FROM inventario.Existencias WITH (UPDLOCK) WHERE ProductoId = @ProductoId;
UPDATE inventario.Existencias SET Piezas = @Stock - @Cantidad WHERE ProductoId = @ProductoId;
COMMIT;
```

Si la lectura y la escritura ocurren en **peticiones HTTP distintas** (el usuario ve el stock en Itzamna.Clio y confirma después), usa [[Concurrencia optimista]] con `rowversion`.

Ver si la base tiene RCSI:

```sql
SELECT name, is_read_committed_snapshot_on, snapshot_isolation_state_desc
FROM   sys.databases WHERE name = DB_NAME();
```

### Cuándo usarlo y cuándo no
- **Es el nivel adecuado** para la mayoría de operaciones de una API.
- **Activa RCSI** si tienes bloqueos entre lecturas y escrituras y puedes asumir el costo del version store (es el comportamiento de Azure SQL).
- **No basta** cuando necesitas que varias lecturas de una transacción vean los mismos datos (usa SNAPSHOT) o cuando el patrón es "verificar y luego escribir" (usa instrucciones atómicas, `UPDLOCK` o restricciones).
- Si con RCSI un caso puntual necesita el comportamiento con bloqueos, existe la pista `WITH (READCOMMITTEDLOCK)`.

### Errores comunes
1. **Creer que READ COMMITTED evita actualizaciones perdidas**: no lo hace.
2. **Suponer que una consulta larga ve un estado consistente** con bloqueos: puede leer filas dos veces o saltarlas si se mueven.
3. **Lógica que depende de que el lector espere**: al activar RCSI, el lector ya no espera y lee el valor anterior.
4. **Usar `NOLOCK` para evitar las esperas** en lugar de RCSI.

### Para profundizar
- [SET TRANSACTION ISOLATION LEVEL](https://learn.microsoft.com/sql/t-sql/statements/set-transaction-isolation-level-transact-sql)
- [Guía de bloqueo y control de versiones de filas](https://learn.microsoft.com/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué garantiza `READ COMMITTED`?
> Que solo lees datos confirmados. No garantiza que, si vuelves a leer la misma fila dentro de la transacción, tenga el mismo valor.

> [!question]- ¿Qué diferencia hay entre READ COMMITTED con bloqueos y con RCSI?
> Con bloqueos, un lector espera si la fila tiene un bloqueo exclusivo. Con RCSI, el lector lee la última versión confirmada **al inicio de la instrucción** y no espera; los escritores siguen bloqueándose entre sí.

> [!question]- Caso: lees el stock, calculas y luego haces `UPDATE` con el valor calculado, en READ COMMITTED. ¿Qué puede fallar?
> Otra sesión puede cambiar el stock entre tu lectura y tu escritura (actualización perdida). Haz la operación atómica (`UPDATE ... SET Stock = Stock - @n WHERE Stock >= @n`) o lee con `UPDLOCK`, o usa concurrencia optimista con `rowversion`.

## Práctica

- [[Lab 5 - Concurrencia optimista con rowversion en dos sesiones]]
- [[Lab 6 - Bloqueos, niveles de aislamiento y deadlock en dos sesiones]]

