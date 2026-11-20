---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Arquitectura y patrones
tags: [arquitectura-patrones]
---
# Patrón Saga

Parte de: [[09-Arquitectura-Patrones/03 Patrones de nube y sistemas distribuidos/_Índice Patrones de nube y sistemas distribuidos|Patrones de nube y sistemas distribuidos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una saga es una forma de mantener la consistencia de una **operación de negocio que abarca varios servicios o bases de datos** sin una transacción distribuida. Se divide en una **secuencia de transacciones locales**, cada una en su servicio; si un paso falla, se ejecutan **transacciones de compensación** que deshacen semánticamente los pasos anteriores. El resultado es consistencia eventual: durante la saga, el sistema puede estar en estados intermedios visibles (vuelo reservado pero aún no cobrado). Sirve cuando cada servicio es dueño de sus datos ([[Microservicios]]) y no puedes (ni quieres) bloquear varias bases con un commit en dos fases.

### Cómo funciona
#### Coreografía vs orquestación
- **Coreografía:** no hay coordinador. Cada servicio hace su paso y **publica un evento**; el siguiente servicio reacciona a ese evento. Si alguien falla, publica un evento de falla y los anteriores compensan. Es muy desacoplada, pero el flujo completo **no está escrito en ningún lugar**: hay que reconstruirlo leyendo suscripciones, y con muchos pasos se vuelve difícil de seguir y de cambiar (riesgo de ciclos).
- **Orquestación:** un **orquestador** (un componente con estado) dice a cada servicio qué hacer mediante comandos, espera respuestas y decide el siguiente paso o las compensaciones. El flujo está **explícito y en un solo lugar**, es más fácil de monitorear y probar; a cambio, el orquestador es un componente más que operar y puede acumular demasiada lógica.

Regla práctica: 2–3 pasos simples, coreografía; flujos con más pasos, bifurcaciones o tiempos de espera, orquestación.

#### Caso: vuelo → hotel → cobro, y el cobro falla
```mermaid
sequenceDiagram
    participant O as Orquestador
    participant V as Vuelos
    participant H as Hoteles
    participant P as Pagos
    O->>V: ReservarVuelo
    V-->>O: VueloReservado (R-881)
    O->>H: ReservarHotel
    H-->>O: HotelReservado (H-42)
    O->>P: Cobrar
    P-->>O: CobroRechazado
    O->>H: CancelarHotel (H-42)
    O->>V: CancelarVuelo (R-881)
    O->>O: Saga = Compensada; notificar al cliente
```
Los pasos 1 y 2 ya están confirmados en sus bases. Al fallar el cobro, el orquestador ejecuta las compensaciones **en orden inverso**: cancela el hotel y luego el vuelo, y marca la saga como fallida. El cliente recibe "no se pudo completar tu reservación: pago rechazado".

#### ¿Una compensación "deshace" exactamente?
**No.** Es un **deshacer semántico**, no un rollback. Las transacciones locales ya se confirmaron y quizá otros las vieron o actuaron sobre ellas:
- Cancelar el hotel puede tener **cargo por cancelación**, o el cuarto pudo venderse a otro y ya no está.
- Si se envió un correo de "vuelo reservado", la compensación es otro correo, no "des-enviarlo".
- Si sí hubo un cobro, la compensación es un **reembolso** (un nuevo movimiento), no borrar el cargo.

Por eso las compensaciones deben diseñarse como operaciones de negocio, ser **idempotentes** y **reintentables** (pueden fallar también), y conviene ordenar la saga para que los pasos difíciles de compensar (el cobro, el envío físico) vayan al final, después del último paso que pueda fallar (*pivot transaction*).

### Ejemplo
Orquestador simple con estado persistido (en producción usarías Durable Functions o una máquina de estados de una librería de mensajería):

```csharp
public enum EstadoSaga { Iniciada, VueloReservado, HotelReservado, Completada, Compensando, Compensada }

public sealed class SagaReservacion(IVuelos vuelos, IHoteles hoteles, IPagos pagos, IRepoSagas repo)
{
    public async Task EjecutarAsync(SagaDatos s, CancellationToken ct)
    {
        try
        {
            s.ReservaVuelo = await vuelos.ReservarAsync(s.Viaje, s.IdempotencyKey, ct);
            await repo.GuardarAsync(s with { Estado = EstadoSaga.VueloReservado }, ct);

            s.ReservaHotel = await hoteles.ReservarAsync(s.Viaje, s.IdempotencyKey, ct);
            await repo.GuardarAsync(s with { Estado = EstadoSaga.HotelReservado }, ct);

            await pagos.CobrarAsync(s.Cliente, s.Total, s.IdempotencyKey, ct); // pivote: al final
            await repo.GuardarAsync(s with { Estado = EstadoSaga.Completada }, ct);
        }
        catch (PagoRechazadoException)
        {
            await repo.GuardarAsync(s with { Estado = EstadoSaga.Compensando }, ct);
            if (s.ReservaHotel is not null) await hoteles.CancelarAsync(s.ReservaHotel, ct); // idempotentes
            if (s.ReservaVuelo is not null) await vuelos.CancelarAsync(s.ReservaVuelo, ct);
            await repo.GuardarAsync(s with { Estado = EstadoSaga.Compensada }, ct);
        }
    }
}
```

Guardar el estado tras cada paso permite **reanudar** la saga si el proceso muere a la mitad, y la llave de idempotencia evita reservar dos veces al reintentar. Los comandos y eventos entre servicios se publican con [[Patrón Outbox|outbox]].

### Cuándo usarlo y cuándo no
- **Úsalo** cuando una operación de negocio cruza servicios con bases propias y necesitas consistencia eventual con recuperación ante fallos.
- **No lo uses** si todo vive en una base: una transacción local de SQL Server es más simple y correcta. Si te encuentras con sagas entre módulos de un mismo monolito con la misma base, revisa los límites.

### Errores comunes
1. **Compensaciones que asumen rollback exacto.**
2. **No persistir el estado** de la saga: una caída deja reservas huérfanas.
3. **Pasos y compensaciones no idempotentes**, duplicados en cada reintento.
4. **Coreografía con muchos pasos** que nadie puede seguir.
5. **Ignorar el aislamiento:** otros ven estados intermedios; usa estados explícitos ("Pendiente de pago") en lugar de mostrar la reserva como confirmada.

### Para profundizar
- [Patrón Saga (Azure Architecture Center)](https://learn.microsoft.com/en-us/azure/architecture/patterns/saga)
- [Saga (microservices.io)](https://microservices.io/patterns/data/saga.html)
- [Durable Functions (Microsoft Learn)](https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una saga?
> Una secuencia de transacciones locales en varios servicios donde cada paso tiene una **acción compensatoria**; si un paso falla, se ejecutan las compensaciones de los anteriores. Sustituye a una transacción distribuida.

> [!question]- Coreografía vs orquestación.
> Coreografía: cada servicio reacciona a eventos de los otros (sin centro; difícil de seguir con muchos pasos). Orquestación: un orquestador envía comandos y lleva el estado (más visible y fácil de cambiar; es un componente más).

> [!question]- Caso: reservar vuelo → hotel → cobrar; el cobro falla. ¿Qué pasa?
> El orquestador ejecuta compensaciones en orden inverso: cancelar hotel, cancelar vuelo. Cada compensación debe ser idempotente y reintentable.

> [!question]- ¿Una compensación "deshace" exactamente?
> No siempre: es una acción semántica (reembolso, correo de disculpa). Mientras la saga corre, otros pueden ver estados intermedios; diseña con eso en mente.

