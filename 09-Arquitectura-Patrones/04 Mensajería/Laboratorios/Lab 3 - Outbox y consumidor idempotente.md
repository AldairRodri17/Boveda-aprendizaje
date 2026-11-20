---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Arquitectura y patrones
herramienta: .NET SDK, SQL Server Express o LocalDB, EF Core
tags: [arquitectura-patrones, laboratorio]
---
# Lab 3 - Outbox y consumidor idempotente

Parte de: [[09-Arquitectura-Patrones/04 Mensajería/_Índice Mensajería|Mensajería]]

**Cubre:** [[Consumidores idempotentes]] · [[Garantías de entrega (at-most, at-least y exactly-once)]] · [[Entrega at-least-once]] · [[Reintentos con backoff]] · [[Dead letter queues y mensajes venenosos]] · [[RabbitMQ]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Implementar el [[Patrón Outbox]] para publicar eventos sin perderlos y un consumidor idempotente que tolere duplicados, reintentos y mensajes venenosos.

## Pasos
1. Crea un API mínimo con EF Core y tablas `Pedidos`, `Outbox(Id, Tipo, Payload, CreadoEn, ProcesadoEn, Intentos)` y, del lado consumidor, `MensajesProcesados(MessageId PK, ProcesadoEn)` y `Facturas`.
2. `POST /pedidos` guarda el pedido **y** un registro `PedidoCreado` en `Outbox` en la **misma** `SaveChangesAsync` (una transacción).
3. Escribe un `BackgroundService` publicador que cada segundo lea hasta 20 registros pendientes de `Outbox`, los "publique" (para empezar, a un `Channel<T>` en memoria; opcional: RabbitMQ en Docker) y marque `ProcesadoEn`.
4. Escribe el consumidor que, al recibir `PedidoCreado`, cree una `Factura`. Hazlo idempotente usando `MensajesProcesados` en la misma transacción que la factura.
5. Simula duplicados: haz que el publicador publique cada mensaje **dos veces**. Verifica que solo existe una factura por pedido.
6. Simula un fallo entre publicar y marcar `ProcesadoEn` (lanza una excepción a propósito una vez). Reinicia y verifica que el mensaje se reenvía y que no hay factura duplicada.
7. Agrega reintentos con backoff en el consumidor (máx. 5) y una tabla o cola `DeadLetters` donde caiga un mensaje con payload inválido tras agotar intentos.
8. Escribe en Notas personales qué garantía de entrega obtuviste y por qué.

## Criterios de terminado
- [ ] Al detener el proceso justo después de `POST /pedidos`, el evento sigue en `Outbox` y se publica al reiniciar.
- [ ] Con publicación doble, `SELECT COUNT(*) FROM Facturas GROUP BY PedidoId` nunca da más de 1.
- [ ] Un mensaje con payload inválido termina en `DeadLetters` con el motivo del error después de 5 intentos, y el resto de mensajes sigue fluyendo.
- [ ] El `MessageId` lo genera el productor (es el `Id` de la fila de Outbox).
- [ ] Puedes explicar por qué el resultado es *at-least-once* + idempotencia = efectivamente una vez.

## Solución

> [!success]- Escritura con outbox (misma transacción)
> ```csharp
> app.MapPost("/pedidos", async (CrearPedido cmd, AppDb db, CancellationToken ct) =>
> {
>     var pedido = new Pedido { Id = Guid.NewGuid(), Total = cmd.Total };
>     db.Pedidos.Add(pedido);
>     db.Outbox.Add(new OutboxMensaje
>     {
>         Id = Guid.NewGuid(), Tipo = "PedidoCreado",
>         Payload = JsonSerializer.Serialize(new { pedido.Id, pedido.Total }),
>         CreadoEn = DateTime.UtcNow
>     });
>     await db.SaveChangesAsync(ct);   // una sola transacción
>     return Results.Created($"/pedidos/{pedido.Id}", pedido);
> });
> ```

> [!success]- Publicador
> ```csharp
> protected override async Task ExecuteAsync(CancellationToken ct)
> {
>     while (!ct.IsCancellationRequested)
>     {
>         using var scope = scopes.CreateScope();
>         var db = scope.ServiceProvider.GetRequiredService<AppDb>();
>         var pendientes = await db.Outbox.Where(m => m.ProcesadoEn == null)
>             .OrderBy(m => m.CreadoEn).Take(20).ToListAsync(ct);
>         foreach (var m in pendientes)
>         {
>             await canal.Writer.WriteAsync(m, ct);   // publicar (puede duplicarse)
>             m.ProcesadoEn = DateTime.UtcNow;         // si falla aquí, se republica: at-least-once
>         }
>         await db.SaveChangesAsync(ct);
>         await Task.Delay(1000, ct);
>     }
> }
> ```
> Con varias instancias del publicador hay que evitar que dos tomen la misma fila (p. ej. `UPDATE ... OUTPUT` con `READPAST`/`UPDLOCK` en SQL Server) o aceptar duplicados porque el consumidor es idempotente.

> [!success]- Consumidor idempotente
> ```csharp
> async Task ManejarAsync(OutboxMensaje m, AppDb db, CancellationToken ct)
> {
>     await using var tx = await db.Database.BeginTransactionAsync(ct);
>     if (await db.MensajesProcesados.AnyAsync(x => x.MessageId == m.Id, ct)) return; // duplicado
>     var datos = JsonSerializer.Deserialize<PedidoCreado>(m.Payload)
>         ?? throw new InvalidDataException("Payload vacío");
>     db.Facturas.Add(new Factura { PedidoId = datos.Id, Total = datos.Total });
>     db.MensajesProcesados.Add(new MensajeProcesado { MessageId = m.Id, ProcesadoEn = DateTime.UtcNow });
>     await db.SaveChangesAsync(ct);   // PK en MessageId protege también ante carreras
>     await tx.CommitAsync(ct);
> }
> ```
> Si dos consumidores procesan el mismo mensaje a la vez, el segundo falla por la PK duplicada; captura esa `DbUpdateException` y trátala como "ya procesado".

> [!success]- Garantía obtenida
> El publicador puede publicar de nuevo (si muere antes de marcar) → **at-least-once**. El consumidor registra el `MessageId` en la misma transacción que el efecto → los duplicados no tienen efecto. Juntos: **efectivamente una vez**.

## Notas personales

_Qué me costó, errores que cometí, dudas._
