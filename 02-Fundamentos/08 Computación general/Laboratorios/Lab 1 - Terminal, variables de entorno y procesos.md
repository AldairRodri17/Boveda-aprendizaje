---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: PowerShell 7 (o Windows PowerShell), Git Bash o WSL
tags: [fundamentos, laboratorio]
contenido: completo
---
# Lab 1 - Terminal, variables de entorno y procesos

Parte de: [[02-Fundamentos/08 Computación general/_Índice Computación general|Computación general]]

**Cubre:** [[Terminal (bash y PowerShell)]] · [[Variables de entorno]] · [[Procesos e hilos]] · [[Linux para desarrolladores (SSH, permisos, procesos y systemd)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Usar la terminal con soltura en bash y PowerShell: redirecciones, códigos de salida, variables de entorno heredadas y manejo de procesos.

## Requisitos
PowerShell (incluido en Windows) y Git Bash o WSL (Ubuntu) para la parte de bash.

## Pasos
1. En bash y en PowerShell crea `app.log` con 5 líneas, dos de ellas con `ERROR`. Cuéntalas y guarda solo las líneas de error en `errores.txt`.
2. Ejecuta un comando que falle (`ls /no-existe` o `Get-Item C:\no-existe`) y muestra su código de salida. Redirige los errores a `err.txt` y la salida normal a `out.txt` en un mismo comando.
3. Define la variable `SALUDO=hola` **sin exportar** en bash y abre un subshell (`bash -c 'echo $SALUDO'`). Repite con `export`. Haz lo equivalente en PowerShell con `$env:SALUDO` y `pwsh -c '$env:SALUDO'` (o `powershell -c`).
4. Lanza un proceso de larga duración (`sleep 300 &` en bash; `Start-Process notepad` o `Start-Job { Start-Sleep 300 }` en PowerShell). Encuéntralo por nombre, obtén su PID y termínalo.
5. En bash, crea un script `saluda.sh` que imprima `$1` y la variable `APP_ENV`, con `set -euo pipefail`. Dale permiso de ejecución solo al dueño y ejecútalo con `APP_ENV=dev ./saluda.sh Ana`. Ejecútalo sin argumento y explica el error.
6. Revisa cuántos hilos tiene un proceso: en PowerShell `(Get-Process -Id $PID).Threads.Count`; en Linux `ps -o nlwp= -p $$`.

## Criterios de terminado
- [ ] `errores.txt` tiene exactamente las 2 líneas con `ERROR` en ambas terminales.
- [ ] Muestras un código de salida distinto de 0 (bash) y `$?` en `False` (PowerShell), y `err.txt` contiene el mensaje de error.
- [ ] Demuestras que sin `export` el subshell no ve la variable y con `export` sí; en PowerShell `$env:` sí se hereda.
- [ ] Terminaste el proceso del paso 4 por su PID y ya no aparece al listarlo.
- [ ] `ls -l saluda.sh` muestra `-rwx------` o `-rwxr--r--` según lo elegido, y sin argumento el script falla con `unbound variable`.

## Solución

> [!success]- Pasos 1–2
> ```bash
> printf 'ok\nERROR a\nok\nERROR b\nok\n' > app.log
> grep -c ERROR app.log && grep ERROR app.log > errores.txt
> ls /no-existe > out.txt 2> err.txt; echo $?      # 2 en GNU ls
> ```
> ```powershell
> "ok","ERROR a","ok","ERROR b","ok" | Set-Content app.log
> (Select-String app.log -Pattern ERROR).Count
> Select-String app.log -Pattern ERROR | ForEach-Object Line | Set-Content errores.txt
> Get-Item C:\no-existe > out.txt 2> err.txt; $?
> ```

> [!success]- Paso 3: herencia de variables
> En bash, una variable sin `export` es solo del shell actual; `export` la pone en el entorno que heredan los procesos hijos. En PowerShell, `$env:SALUDO = 'hola'` modifica el entorno del proceso, así que cualquier hijo lanzado después la ve; una variable normal (`$saludo`) no se hereda.

> [!success]- Pasos 4–6
> ```bash
> sleep 300 & pgrep -a sleep; kill <pid>      # SIGTERM; kill -9 solo si no responde
> chmod 700 saluda.sh   # o chmod u+x
> ```
> ```powershell
> Start-Process notepad; Get-Process notepad | Select-Object Id, Name; Stop-Process -Id <id>
> ```
> Con `set -u`, usar `$1` sin argumento detiene el script con `$1: unbound variable`; es mejor que seguir con un valor vacío. Un proceso tiene al menos un hilo; .NET y PowerShell suelen mostrar varios (GC, thread pool, etc.).

## Notas personales

_Qué me costó, errores que cometí, dudas._
