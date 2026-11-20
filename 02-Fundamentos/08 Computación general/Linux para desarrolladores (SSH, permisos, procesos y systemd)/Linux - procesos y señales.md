---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Linux - procesos y señales

Parte de: [[Linux para desarrolladores (SSH, permisos, procesos y systemd)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **proceso** es un programa en ejecución, identificado por un número único, el **PID**. Cada proceso tiene un **padre** (PPID), un usuario, un estado y recursos asignados. Las **señales** son notificaciones asíncronas que el kernel u otros procesos envían a un proceso para pedirle algo: que termine, que recargue configuración, que se detenga. Saber manejarlos te permite diagnosticar una API colgada o que consume demasiada CPU, y apagarla sin perder peticiones.

### Cómo funciona
**Ver procesos:**

```bash
ps aux                              # todos los procesos con usuario, %CPU, %MEM y comando
ps aux | grep -i dionisio           # buscar uno
pgrep -af dotnet                    # PID y línea de comando de los que coinciden
top                                 # vista en vivo (q para salir); htop si está instalado
ps -o pid,ppid,user,%cpu,%mem,cmd -p 1234
```

**Señales principales:**

| Señal | Número | Significado | ¿Se puede atrapar? |
|---|---|---|---|
| `SIGTERM` | 15 | "termina, por favor": la petición estándar de apagado | sí: la app puede cerrar ordenadamente |
| `SIGINT` | 2 | interrupción (Ctrl+C en la terminal) | sí |
| `SIGKILL` | 9 | el kernel elimina el proceso de inmediato | **no**: ni se entera |
| `SIGHUP` | 1 | se cerró la terminal; muchos daemons lo usan para recargar configuración | sí |
| `SIGSTOP`/`SIGCONT` | 19/18 (en x86) | pausar / reanudar | `SIGSTOP` no |

```bash
kill 1234            # envía SIGTERM (por defecto)
kill -TERM 1234      # lo mismo, explícito
kill -9 1234         # SIGKILL: último recurso
pkill -f Dionisio    # por nombre o línea de comando
```

**`SIGTERM` vs `SIGKILL`:** con `SIGTERM`, una app ASP.NET Core recibe la señal, el host dispara `IHostApplicationLifetime.ApplicationStopping`, deja de aceptar conexiones nuevas, termina las peticiones en curso y llama `StopAsync` en los `BackgroundService` (con un tiempo límite configurable con `HostOptions.ShutdownTimeout`). Con `SIGKILL` nada de eso pasa: conexiones cortadas, transacciones a medias que la base deberá deshacer, mensajes de cola sin confirmar, logs sin escribir. Por eso el flujo correcto es `SIGTERM`, esperar, y solo si no termina, `SIGKILL`. Es exactamente lo que hacen `systemctl stop`, `docker stop` (espera 10 s por defecto) y Kubernetes (30 s por defecto).

**PID 1 en contenedores:** dentro de un contenedor, el primer proceso tiene PID 1 y es tratado de forma especial por el kernel: **no recibe el comportamiento por defecto de las señales** (si no instala un manejador, un `SIGTERM` simplemente se ignora) y debe **recoger a los procesos hijos** que terminan (si no, quedan "zombis"). Consecuencias:
- Si en el Dockerfile usas la forma *shell* (`ENTRYPOINT dotnet app.dll`), el PID 1 es `/bin/sh`, que normalmente **no reenvía** `SIGTERM` a tu app; al detener el contenedor se agota el tiempo de gracia y llega `SIGKILL`. Usa la forma *exec*: `ENTRYPOINT ["dotnet", "Itzamna.Dionisio.Api.dll"]`.
- El runtime de .NET sí maneja `SIGTERM` como PID 1, así que con la forma exec el apagado ordenado funciona.
- Si tu proceso lanza hijos, usa un init mínimo: `docker run --init` (usa tini).

### Ejemplo
La API en una VM consume 100% de CPU:

```bash
pgrep -af Itzamna.Dionisio                 # obtener el PID, digamos 4321
top -p 4321                                # confirmar consumo
dotnet-counters monitor -p 4321            # métricas del runtime (si tienes las herramientas)
dotnet-dump collect -p 4321                # volcado para analizar después
sudo systemctl restart dionisio            # reinicio ordenado vía systemd (SIGTERM y luego SIGKILL si no responde)
```

Apagado ordenado en código:

```csharp
builder.Services.Configure<HostOptions>(o => o.ShutdownTimeout = TimeSpan.FromSeconds(20));

app.Lifetime.ApplicationStopping.Register(() =>
    app.Logger.LogInformation("SIGTERM recibido: terminando peticiones en curso"));
```

### Cuándo usarlo y cuándo no
- Usa `SIGTERM` siempre primero; `SIGKILL` solo si el proceso no responde después del tiempo de gracia.
- Para servicios administrados por systemd, usa `systemctl` en vez de `kill`: si matas el proceso a mano, systemd puede reiniciarlo según `Restart=` (ver [[Linux - servicios con systemd]]).
- En contenedores, alinea el tiempo de gracia del orquestador con `ShutdownTimeout` de la app.

### Errores comunes
- **`kill -9` por costumbre**: pierdes trabajo y no pruebas tu apagado ordenado.
- **Forma shell en `ENTRYPOINT`/`CMD`**: la app nunca recibe `SIGTERM`.
- **`BackgroundService` que ignora el `CancellationToken`**: el host espera hasta el timeout y luego se corta.
- **Matar el proceso equivocado** con `pkill` y un patrón amplio: verifica con `pgrep -af` antes.
- **Confundir "proceso colgado" con "proceso esperando E/S"**: revisa `top` (estado `D` es espera de disco) antes de matarlo.

### Para profundizar
- [signal(7): descripción de las señales en Linux](https://man7.org/linux/man-pages/man7/signal.7.html)
- [Docker: referencia de docker container run (opción --init)](https://docs.docker.com/reference/cli/docker/container/run/)
- [Host genérico de .NET: apagado del host](https://learn.microsoft.com/dotnet/core/extensions/generic-host)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo listas procesos y buscas uno?
> `ps aux`, `ps aux | grep dotnet`, `pgrep -a dotnet`; en vivo con `top` o `htop`.

> [!question]- ¿Qué diferencia hay entre `SIGTERM` y `SIGKILL`?
> `SIGTERM` (15, lo que manda `kill` por defecto) pide terminar y permite limpiar: ASP.NET Core lo atiende con apagado ordenado. `SIGKILL` (9) mata al instante sin oportunidad de limpiar.

> [!question]- ¿Por qué importa el PID 1 en contenedores?
> El proceso PID 1 recibe las señales de `docker stop`. Si es un shell que no las reenvía, la app no se entera y termina con `SIGKILL` tras el timeout. Usa la forma exec en `ENTRYPOINT ["dotnet", "app.dll"]`.

