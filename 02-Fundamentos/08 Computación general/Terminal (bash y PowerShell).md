---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Terminal (bash y PowerShell)

Parte de: [[02-Fundamentos/08 Computación general/_Índice Computación general|Computación general]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **terminal** es la interfaz de texto para darle órdenes al sistema. El programa que interpreta esas órdenes es el **shell**. Los dos que más vas a usar:

- **bash**: el shell estándar en Linux (servidores, contenedores, agentes de Azure Pipelines con Ubuntu, WSL) y disponible en Windows con Git Bash.
- **PowerShell**: el shell de Microsoft. **PowerShell 7+** (`pwsh`) es multiplataforma (Windows, Linux, macOS); Windows PowerShell 5.1 viene preinstalado en Windows.

Saber ambos te permite automatizar tareas, revisar logs, depurar servidores y escribir pasos de pipeline sin depender de interfaces gráficas.

### Cómo funciona
**La diferencia fundamental está en la tubería (`|`):**
- En **bash**, cada comando es un programa que lee y escribe **texto** (bytes). La tubería pasa la salida de texto de uno a la entrada del siguiente, y tú lo recortas con `grep`, `awk`, `cut`, `sort`.
- En **PowerShell**, los cmdlets emiten **objetos .NET** con propiedades. La tubería pasa objetos, así que filtras y ordenas por propiedad (`Where-Object`, `Sort-Object`, `Select-Object`) sin parsear texto.

**Redirecciones** (iguales en ambos para lo básico):

| Operador | Efecto |
|---|---|
| `>` | manda la salida estándar (stdout, descriptor 1) a un archivo, **sobrescribiendo** |
| `>>` | igual, pero **agrega** al final |
| `2>` | manda los **errores** (stderr, descriptor 2) a un archivo |
| `2>&1` | manda stderr **al mismo lugar que stdout** |
| `< archivo` | usa un archivo como entrada (bash) |

Ejemplo: `dotnet build > build.log 2>&1` guarda salida y errores juntos. El orden importa en bash: `2>&1 > archivo` no hace lo mismo.

**¿Falló el último comando?**
- bash: `$?` contiene el **código de salida** del último comando (0 = éxito, distinto de 0 = error). `cmd1 && cmd2` corre `cmd2` solo si `cmd1` tuvo éxito; `cmd1 || cmd2`, solo si falló. En scripts, `set -euo pipefail` hace que el script se detenga ante errores.
- PowerShell: `$?` es `True`/`False` para el último comando, y `$LASTEXITCODE` guarda el código de salida del último **programa externo** (como `dotnet` o `git`). Para cmdlets, `-ErrorAction Stop` convierte errores en excepciones atrapables con `try/catch`. PowerShell 7 también admite `&&` y `||`.

### Ejemplo
Tareas típicas en ambos shells:

```bash
 # bash: líneas con "Warn" en el log de la API (grep -c cuenta, -i ignora mayúsculas)
grep -c "Warn" api.log
grep -i "timeout" api.log | tail -n 20          # últimas 20 coincidencias

 # Archivos *.sql que mencionan la tabla Pedidos, recursivo, solo nombres
grep -rl --include="*.sql" "dbo.Pedidos" src/

 # Tamaño de carpetas y procesos que más CPU usan
du -sh */ | sort -h
ps aux --sort=-%cpu | head -n 6
```

```powershell
 # PowerShell: lo mismo con objetos
(Select-String -Path api.log -Pattern 'Warn' -CaseSensitive).Count
Select-String -Path api.log -Pattern 'timeout' | Select-Object -Last 20

Get-ChildItem -Path src -Recurse -Filter *.sql |
    Select-String -Pattern 'dbo.Pedidos' -List |
    Select-Object -ExpandProperty Path

 # Los 5 procesos que más CPU acumulan
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5 Name, Id, CPU
```

Observa que `Select-String` **no distingue mayúsculas por defecto** (de ahí `-CaseSensitive`), mientras que `grep` **sí distingue** salvo que uses `-i`. Y que para ordenar procesos en PowerShell usas la propiedad (`CPU`, `WS` para memoria) en lugar de una columna de texto.

Script de paso de pipeline que se detiene ante errores:

```bash
#!/usr/bin/env bash
set -euo pipefail
dotnet restore
dotnet build -c Release --no-restore
dotnet test -c Release --no-build || { echo "Pruebas fallidas"; exit 1; }
```

### Cuándo usarlo y cuándo no
- **bash** en servidores Linux, contenedores, scripts de pipeline en agentes Ubuntu y WSL.
- **PowerShell** para automatizar Windows, Azure (módulo `Az`) y cuando quieres manipular datos estructurados (JSON con `ConvertFrom-Json`, CSV con `Import-Csv`).
- Para scripts que deben correr en cualquier plataforma, `pwsh` es buena opción; también lo son scripts en C# (`dotnet run app.cs` en .NET 10) o Python cuando la lógica crece.
- Un script de más de ~100 líneas con lógica compleja suele estar mejor en un lenguaje de programación.

### Errores comunes
- **Comillas**: en bash, `'...'` no expande variables y `"..."` sí; en PowerShell es igual con `$variable`. Rutas con espacios siempre entre comillas.
- **No revisar códigos de salida** en scripts: el pipeline "pasa" aunque `dotnet test` falle. Usa `set -e` o revisa `$LASTEXITCODE`.
- **Fin de línea CRLF** en scripts bash creados en Windows: `/usr/bin/env: 'bash\r'`. Configura `.gitattributes` con `*.sh text eol=lf`.
- **Asumir que los alias son los mismos**: en PowerShell `curl` y `ls` son alias (en Windows PowerShell 5.1 `curl` era `Invoke-WebRequest`); en scripts usa los nombres completos.
- **`rm -rf $VAR/`** con la variable vacía: borra desde la raíz. Usa `set -u` y `${VAR:?}`.

### Para profundizar
- [Manual de GNU Bash](https://www.gnu.org/software/bash/manual/bash.html)
- [Documentación de PowerShell (Microsoft Learn)](https://learn.microsoft.com/powershell/scripting/overview)
- [Acerca de las redirecciones en PowerShell](https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_redirection)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Cuenta cuántas líneas de `app.log` contienen `ERROR` (bash y PowerShell).
> bash: `grep -c ERROR app.log`. PowerShell: `(Select-String -Path app.log -Pattern ERROR).Count`.

> [!question]- Muestra los 5 procesos que más memoria usan en PowerShell.
> `Get-Process | Sort-Object WorkingSet64 -Descending | Select-Object -First 5 Name, WorkingSet64`

> [!question]- Busca recursivamente archivos `*.cs` que contengan `TODO` en bash.
> `grep -rn --include='*.cs' TODO .`

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia fundamental hay entre la tubería (`|`) de bash y la de PowerShell?
> En bash pasan **texto** entre programas; en PowerShell pasan **objetos** .NET con propiedades (`Get-Process | Where-Object CPU -gt 100 | Select-Object Name`).

> [!question]- ¿Qué hacen `>`, `>>`, `2>` y `2>&1`?
> `>` redirige la salida a un archivo (sobrescribe), `>>` agrega, `2>` redirige errores y `2>&1` envía los errores al mismo destino que la salida.

> [!question]- ¿Cómo sabes si el último comando falló?
> bash: `$?` (0 = éxito). PowerShell: `$?` (booleano) y `$LASTEXITCODE` para programas externos. En scripts bash, `set -euo pipefail` detiene ante errores.

## Práctica

- [[Lab 1 - Terminal, variables de entorno y procesos]]

