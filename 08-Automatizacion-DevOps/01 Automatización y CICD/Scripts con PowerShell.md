---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
---
# Scripts con PowerShell

Parte de: [[08-Automatizacion-DevOps/01 Automatización y CICD/_Índice Automatización y CICD|Automatización y CICD]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**PowerShell** es el shell y lenguaje de scripting de Microsoft. La versión moderna, **PowerShell 7** (`pwsh`), es multiplataforma (Windows, Linux, macOS) y se basa en .NET; la antigua **Windows PowerShell 5.1** (`powershell.exe`) sigue incluida en Windows. Es la herramienta natural para administrar Windows, IIS, Azure (módulo `Az`) y Azure DevOps.

### Cómo funciona
**La diferencia clave frente a bash: el pipeline pasa objetos, no texto.** En bash, `ls | grep` mueve líneas de texto que tienes que recortar con `awk` o `cut`. En PowerShell, `Get-ChildItem` produce objetos `FileInfo` con propiedades (`Name`, `Length`, `LastWriteTime`), y el siguiente comando las usa directamente: `Where-Object Length -gt 1MB`. Esto evita parsear texto y hace los scripts más robustos.

Otras características:
- Comandos con forma **Verbo-Sustantivo** (`Get-Item`, `Remove-Item`, `Invoke-RestMethod`) y parámetros con nombre.
- **`-WhatIf`** en comandos que modifican: muestra qué haría sin hacerlo. Ideal para simular borrados.
- **Manejo de errores**: hay errores *terminantes* (detienen) y *no terminantes* (solo avisan y siguen). Por defecto, muchos errores son no terminantes, así que el script continúa. Con `$ErrorActionPreference = 'Stop'` al inicio **todos se vuelven terminantes** y se atrapan con `try { } catch { }`. Para programas externos (`dotnet`, `git`) revisa `$LASTEXITCODE`; en PowerShell 7.3+ existe `$PSNativeCommandUseErrorActionPreference = $true` para que también detengan el script.
- **`Invoke-RestMethod`** llama a una API REST y convierte el JSON de la respuesta en objetos automáticamente.

### Ejemplo
**Los 5 archivos más grandes de una carpeta, en MB**:

```powershell
Get-ChildItem -Path C:\logs -File -Recurse |
    Sort-Object Length -Descending |
    Select-Object -First 5 Name,
        @{ Name = 'MB'; Expression = { [math]::Round($_.Length / 1MB, 2) } }
```

**Borrar `.log` de más de 30 días, simulando primero**:

```powershell
$limite = (Get-Date).AddDays(-30)
$viejos = Get-ChildItem -Path C:\logs -Filter *.log -File |
    Where-Object LastWriteTime -lt $limite

$viejos | Remove-Item -WhatIf      # 1) revisa la lista
# $viejos | Remove-Item            # 2) cuando estés seguro, quita -WhatIf
```

**Health check que sale con código 1 si `/health` no responde 200**:

```powershell
param([Parameter(Mandatory)][string]$Url)
$ErrorActionPreference = 'Stop'
try {
    $resp = Invoke-WebRequest -Uri "$Url/health" -TimeoutSec 10 -UseBasicParsing
    if ($resp.StatusCode -ne 200) { throw "Código $($resp.StatusCode)" }
    Write-Host "Sano"
    exit 0
}
catch {
    Write-Error "Health check falló: $_" -ErrorAction Continue
    exit 1
}
```

**Llamar a una API REST y leer un campo** (la [[Azure DevOps REST API]] con un PAT guardado en una variable secreta):

```powershell
$org = 'mi-organizacion'
$pat = $env:AZDO_PAT                       # nunca escrito en el script
$basic = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(":$pat"))
$uri = "https://dev.azure.com/$org/_apis/projects?api-version=7.1"

$respuesta = Invoke-RestMethod -Uri $uri -Headers @{ Authorization = "Basic $basic" }
$respuesta.value | ForEach-Object { $_.name }   # JSON ya convertido a objetos
```

En pipelines usa la tarea `PowerShell@2` con `pwsh: true` o el atajo `- pwsh:` para ejecutar PowerShell 7 también en agentes Linux.

### Cuándo usarlo y cuándo no
- **Úsalo** para administrar Windows Server e IIS, tareas de Azure con `Az`, y scripts que manipulan objetos (CSV, JSON, XML).
- **Prefiere [[Scripts con bash]]** dentro de contenedores Linux mínimos donde `pwsh` no está instalado.
- **Prefiere [[Scripts con Python]]** para procesamiento de datos con librerías específicas.

### Errores comunes
1. **Error común en pipelines con `pwsh`**: un comando externo falla (por ejemplo `dotnet test` devuelve 1) pero el paso queda verde, porque PowerShell no detiene el script por códigos de salida de programas nativos. Revisa `$LASTEXITCODE` después de cada uno (`if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }`) o activa `$PSNativeCommandUseErrorActionPreference`. También, en `PowerShell@2` sin `pwsh: true` en Windows corre Windows PowerShell 5.1, con diferencias de sintaxis.
2. **No usar `$ErrorActionPreference = 'Stop'`**: errores no terminantes pasan desapercibidos.
3. **Borrar sin `-WhatIf`** primero.
4. **Política de ejecución** en Windows bloquea scripts descargados: usa `-ExecutionPolicy Bypass` solo en contextos controlados o firma tus scripts.
5. **Secretos en texto plano** en el script; usa variables secretas del pipeline, `SecretManagement` o [[Key Vault]].

### Para profundizar
- [Documentación de PowerShell](https://learn.microsoft.com/powershell/)
- [about_Preference_Variables (ErrorActionPreference)](https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_preference_variables)
- [Invoke-RestMethod](https://learn.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-restmethod)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Lista los 5 archivos más grandes de una carpeta con nombre y tamaño en MB.
> ```powershell
> Get-ChildItem C:\logs -File -Recurse |
>   Sort-Object Length -Descending |
>   Select-Object -First 5 Name, @{n='MB'; e={[math]::Round($_.Length/1MB, 2)}}
> ```

> [!question]- Borra los archivos `.log` de más de 30 días (primero simula).
> ```powershell
> Get-ChildItem C:\logs -Filter *.log |
>   Where-Object LastWriteTime -lt (Get-Date).AddDays(-30) |
>   Remove-Item -WhatIf    # quita -WhatIf cuando estés seguro
> ```

> [!question]- Haz un health check que salga con código 1 si `/health` no responde 200.
> ```powershell
> try {
>   $r = Invoke-WebRequest https://mi-api.azurewebsites.net/health -TimeoutSec 10
>   if ($r.StatusCode -ne 200) { exit 1 }
> } catch { Write-Error $_; exit 1 }
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace especial a PowerShell frente a bash?
> El pipeline pasa **objetos .NET**, no texto: `Get-Process | Where-Object CPU -gt 100 | Select-Object Name, CPU`. Además PowerShell 7 (`pwsh`) es multiplataforma.

> [!question]- ¿Cómo haces que un script se detenga ante el primer error?
> `$ErrorActionPreference = 'Stop'` al inicio (y `Set-StrictMode -Version Latest`). Con `try { } catch { }` manejas el error; sin eso, muchos errores son *no terminantes* y el script sigue.

> [!question]- ¿Cómo llamas a una API REST y lees un campo?
> `$r = Invoke-RestMethod -Uri https://api.ejemplo.com/items -Headers @{ Authorization = "Bearer $token" }` y luego `$r.items[0].name`: la respuesta JSON ya viene convertida en objeto.

> [!question]- Error común en pipelines con `pwsh`.
> Un comando nativo (p. ej. `dotnet`) que falla no lanza excepción: revisa `$LASTEXITCODE` o, en PowerShell 7.4+, usa `$PSNativeCommandUseErrorActionPreference = $true`.

