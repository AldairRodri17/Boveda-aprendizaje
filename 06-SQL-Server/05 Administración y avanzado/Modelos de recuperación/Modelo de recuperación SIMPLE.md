---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: SQL Server
tags: [sql-server]
---
# Modelo de recuperación SIMPLE

Parte de: [[Modelos de recuperación]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
En el modelo **SIMPLE** SQL Server **trunca el log automáticamente** en cada *checkpoint*: en cuanto los registros ya no se necesitan para transacciones activas ni para recuperar tras un reinicio, el espacio del log se reutiliza. No existen backups de log; solo puedes hacer backups **completos** y **diferenciales**. Es el modelo más sencillo de mantener porque el log no crece sin control.

### Cómo funciona
```mermaid
flowchart LR
  T[Transacción confirmada] --> CP[Checkpoint: páginas sucias a disco]
  CP --> TR[El log activo se trunca y se reutiliza]
  TR --> T
```
El log sigue existiendo y sigue siendo indispensable: garantiza la atomicidad (rollback) y la recuperación tras un reinicio inesperado. Lo que cambia es que no se **conserva** para backups. Por eso el log puede crecer solo temporalmente, por ejemplo con una transacción muy grande (un `DELETE` de 50 millones de filas sin lotes) que mantiene activo todo su log hasta terminar.

**Lo que pierdes**: la restauración a un punto en el tiempo y cualquier cambio posterior al último backup. Si tu último completo fue a las 00:00 y el diferencial a las 12:00, una falla a las 17:30 te hace perder 5.5 horas de datos. Tampoco puedes usar log shipping ni Always On availability groups.

### Ejemplo
```sql
-- Ver y cambiar el modelo
SELECT name, recovery_model_desc FROM sys.databases;
ALTER DATABASE Tienda_Dev SET RECOVERY SIMPLE;

-- Estrategia típica en SIMPLE: completo diario y, si la base es grande, diferenciales
BACKUP DATABASE Tienda_Dev TO DISK = N'D:\Backups\Tienda_Dev_full.bak' WITH INIT, COMPRESSION;
BACKUP DATABASE Tienda_Dev TO DISK = N'D:\Backups\Tienda_Dev_diff.bak' WITH DIFFERENTIAL, INIT, COMPRESSION;

-- Esto falla en SIMPLE (no hay backups de log):
-- BACKUP LOG Tienda_Dev TO DISK = N'D:\Backups\x.trn';
-- Msg 4208: The statement BACKUP LOG is not allowed while the recovery model is SIMPLE.
```
Si el log de una base en SIMPLE crece, revisa `log_reuse_wait_desc`: normalmente será `ACTIVE_TRANSACTION` (una transacción larga) o `CHECKPOINT`. La solución es partir las operaciones grandes en lotes, no reducir el archivo cada noche.

Para tu laptop con Docker o SQL Server Developer, donde recreas la base con los scripts de migración, SIMPLE es lo natural.

### Cuándo usarlo y cuándo no
**Apropiado** cuando perder los cambios desde el último backup es aceptable o los datos se pueden regenerar:

- Bases de desarrollo, pruebas y demos.
- *Staging* de ETL y data marts que se reconstruyen desde la fuente (por ejemplo, tablas de carga de Wee.Reporteria).
- Bases de solo lectura o que cambian poco y se respaldan después de cada carga.

**No apropiado** para datos transaccionales de producción (pedidos, pagos, usuarios), donde una pérdida de horas es inaceptable; ahí usa [[Modelo de recuperación FULL]]. Compara también con [[Modelo de recuperación BULK_LOGGED]] y el resumen en [[Modelos de recuperación]].

### Errores comunes
1. **Dejar producción en SIMPLE porque "el log se llenaba".** Resolviste el síntoma perdiendo la restauración puntual; lo correcto era programar backups de log.
2. **Creer que SIMPLE significa "sin log".** Las transacciones grandes siguen necesitando espacio de log; divide los cambios masivos.
3. **Sin diferenciales en una base grande.** Si el completo tarda horas, solo lo harás una vez por noche y la ventana de pérdida será amplia.
4. **Cambiar a SIMPLE y olvidar decírselo al equipo.** Alguien asumirá que puede restaurar a las 10:15 y no podrá.

### Para profundizar
- [Modelos de recuperación (Microsoft Learn)](https://learn.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server)
- [El log de transacciones](https://learn.microsoft.com/sql/relational-databases/logs/the-transaction-log-sql-server)
- [Checkpoints de base de datos](https://learn.microsoft.com/sql/relational-databases/logs/database-checkpoints-sql-server)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace el modelo SIMPLE con el log?
> Reutiliza el espacio del log tras cada *checkpoint* (cuando las transacciones ya no lo necesitan). No hay backups de log.

> [!question]- ¿Qué pierdes en SIMPLE?
> La restauración a un punto en el tiempo: solo puedes volver al último backup completo o diferencial. Todo lo posterior se pierde.

> [!question]- ¿Cuándo es apropiado?
> Bases de desarrollo, pruebas, staging de ETL o datos que puedes reconstruir. En producción con datos importantes, normalmente FULL.

