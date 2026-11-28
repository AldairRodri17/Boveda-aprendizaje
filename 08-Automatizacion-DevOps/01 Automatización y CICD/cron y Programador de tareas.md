---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
contenido: completo
---
# cron y Programador de tareas

Parte de: [[08-Automatizacion-DevOps/01 Automatización y CICD/_Índice Automatización y CICD|Automatización y CICD]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**cron** es el programador de tareas clásico de Linux/Unix: un servicio (*daemon*) que lee una tabla (**crontab**) y ejecuta comandos en los horarios indicados. En Windows, el equivalente es el **Programador de tareas** (*Task Scheduler*). Ambos sirven para respaldos, limpieza de archivos, reportes nocturnos o cualquier script que deba correr solo. La sintaxis de cron se volvió un estándar que reutilizan GitHub Actions (`schedule`), Kubernetes (`CronJob`) y, con un campo extra, Azure Functions ([[Azure Functions programadas]]).

### Cómo funciona
Una línea de crontab tiene **5 campos de tiempo** y luego el comando:

```text
┌───────── minuto (0-59)
│ ┌─────── hora (0-23)
│ │ ┌───── día del mes (1-31)
│ │ │ ┌─── mes (1-12)
│ │ │ │ ┌─ día de la semana (0-7; 0 y 7 = domingo, 1 = lunes)
│ │ │ │ │
* * * * *  comando
```

Operadores: `*` (cualquiera), `,` (lista: `1,15`), `-` (rango: `1-5`), `/` (intervalo: `*/15`).

Ejemplos resueltos:

| Necesidad | Expresión |
|---|---|
| Cada 15 minutos | `*/15 * * * *` |
| 02:00 de lunes a viernes | `0 2 * * 1-5` |
| 23:30 del primer día de cada mes | `30 23 1 * *` |
| Cada hora en punto de 8 a 20 h, sábado y domingo | `0 8-20 * * 6,0` |

**Trampa de día del mes y día de la semana**: si **ambos** campos tienen valores (no `*`), cron ejecuta cuando se cumple **cualquiera** de los dos (es un «o», no un «y»). Así, `0 9 1 * 1` corre a las 09:00 **el día 1 de cada mes y además todos los lunes**, no «el día 1 solo si cae en lunes».

El comando corre con el **entorno mínimo** de cron: un `PATH` corto (típicamente `/usr/bin:/bin`), sin tus variables de `.bashrc`, con el directorio de trabajo en tu home y con `/bin/sh` como shell. Por eso un script que funciona a mano puede fallar en cron.

### Ejemplo
Editar el crontab del usuario con `crontab -e` y agregar el respaldo diario a las 03:15 con salida a un log:

```bash
# m  h  dom mes dow  comando
15  3  *   *   *    /opt/scripts/respaldo.sh >> /var/log/respaldo.log 2>&1
```

`>>` agrega al archivo y `2>&1` manda también los errores al mismo log; sin eso, la salida de error se pierde o se intenta enviar por correo local.

Buenas prácticas dentro del script: rutas absolutas, `set -euo pipefail`, cargar las variables que necesite (`source /opt/scripts/.env`), y usar `flock` para evitar que dos ejecuciones se encimen:

```bash
*/15 * * * * flock -n /tmp/sync.lock /opt/scripts/sincronizar.sh >> /var/log/sync.log 2>&1
```

**Causas típicas de «funciona a mano pero no en cron»**: `PATH` distinto (no encuentra `dotnet`, `az` o `sqlcmd`; usa rutas completas), variables de entorno ausentes, rutas relativas al directorio actual, permisos de ejecución, el `%` (en crontab significa salto de línea; escápalo como `\%`, por ejemplo en `date +\%F`), fin de línea CRLF, o la zona horaria del servidor distinta a la que suponías.

**En Windows** se crea una tarea desde la línea de comandos con `schtasks` o con PowerShell:

```powershell
# schtasks (cmd o PowerShell)
schtasks /Create /TN "Respaldo Dionisio" /SC DAILY /ST 03:15 `
  /TR "pwsh -File C:\scripts\respaldo.ps1" /RU SYSTEM

# PowerShell con objetos
$accion  = New-ScheduledTaskAction -Execute 'pwsh.exe' -Argument '-File C:\scripts\respaldo.ps1'
$disparo = New-ScheduledTaskTrigger -Daily -At 3:15am
Register-ScheduledTask -TaskName 'Respaldo Dionisio' -Action $accion -Trigger $disparo -User 'SYSTEM'
```

### Cuándo usarlo y cuándo no
- **Úsalo** en un servidor propio o VM para tareas locales simples.
- **En la nube prefiere servicios administrados** con logs, alertas y reintentos: [[Azure Functions programadas]], pipelines con `schedules:`, Logic Apps o Elastic Jobs para SQL. Un cron en una VM muere en silencio si la VM se apaga.
- Si la tarea debe correr una sola vez aunque haya varios servidores, cron en cada uno la ejecutaría varias veces.

### Errores comunes
1. **No redirigir la salida**: no hay forma de saber por qué falló.
2. **Olvidar la zona horaria** del servidor (muchas VM en la nube usan UTC).
3. **Tareas que se enciman** porque la anterior aún no termina: usa `flock`.
4. **Confiar en que corrió**: monitorea con un *heartbeat* (el script avisa a un endpoint al terminar y alertas si no llega).

### Para profundizar
- [crontab(5): formato del archivo](https://man7.org/linux/man-pages/man5/crontab.5.html)
- [schtasks (Microsoft Learn)](https://learn.microsoft.com/windows-server/administration/windows-commands/schtasks)
- [Register-ScheduledTask](https://learn.microsoft.com/powershell/module/scheduledtasks/register-scheduledtask)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Cada 15 minutos.
> `*/15 * * * *`

> [!question]- A las 02:00 de lunes a viernes.
> `0 2 * * 1-5`

> [!question]- A las 23:30 del primer día de cada mes.
> `30 23 1 * *`

> [!question]- Cada hora en punto entre 8 y 20 h, solo sábados y domingos.
> `0 8-20 * * 6,0`

> [!question]- Escribe la línea de crontab que corre `/opt/scripts/respaldo.sh` a las 03:15 diario y guarda la salida en un log.
> `15 3 * * * /opt/scripts/respaldo.sh >> /var/log/respaldo.log 2>&1`

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuáles son los 5 campos de cron?
> `minuto hora día-del-mes mes día-de-la-semana` (domingo = 0 o 7). Se editan con `crontab -e`.

> [!question]- Tu script funciona a mano pero falla en cron. ¿Causas típicas?
> Cron corre con un **PATH mínimo** y sin tu perfil: usa rutas absolutas, define variables en el script y redirige salida a un log (`>> /var/log/tarea.log 2>&1`).

> [!question]- ¿Cómo creas una tarea en Windows desde la línea de comandos?
> `schtasks /create /tn "Limpieza" /tr "pwsh -File C:\scripts\limpiar.ps1" /sc daily /st 02:00` o con `Register-ScheduledTask`. Para que corra sin sesión iniciada: «Run whether user is logged on or not» con una cuenta de servicio.

> [!question]- Trampa: `0 9 1 * 1`. ¿Cuándo corre?
> A las 9:00 del **día 1 de cada mes y también cada lunes**: si día del mes y día de la semana están restringidos, cron usa **O**, no Y.

