---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
contenido: completo
---
# Variables de entorno

Parte de: [[02-Fundamentos/08 Computación general/_Índice Computación general|Computación general]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **variable de entorno** es un par **nombre=valor** que el sistema operativo entrega a cada proceso cuando arranca. La app la lee al iniciar para configurarse **sin cambiar el código ni el binario**: en qué ambiente corre, a qué base se conecta, qué nivel de log usar. Es la forma estándar de configurar la misma imagen o artefacto en desarrollo, QA y producción (principio "config" de *The Twelve-Factor App*).

### Cómo funciona
**Herencia:** cada proceso recibe una **copia** del entorno de su proceso padre en el momento en que se crea. Si en una terminal haces `export X=1`, solo esa terminal y los procesos que lance **después** la ven. Otra terminal abierta, el IDE o un servicio ya arrancado tienen su propia copia y no se enteran. Para que algo persista hay que definirlo en un lugar que lean los procesos al nacer: el perfil del shell (`~/.bashrc`), las variables de usuario/sistema de Windows (y reiniciar la app o la terminal), el archivo de servicio de systemd, la configuración del App Service o el `launchSettings.json` en desarrollo.

Definirlas:

| | bash | PowerShell |
|---|---|---|
| Solo para un comando | `VAR=valor comando` | (no hay forma directa; se define y luego se quita) |
| Para la sesión actual | `export VAR=valor` | `$env:VAR = "valor"` |
| Leer | `echo "$VAR"` | `$env:VAR` |
| Listar | `env` / `printenv` | `Get-ChildItem Env:` |
| Persistente | `~/.bashrc`, `/etc/environment` | `[Environment]::SetEnvironmentVariable("VAR","valor","User")` |

**En .NET**, el host lee variables de entorno como un proveedor más de configuración (además de `appsettings.json`, User Secrets y línea de comandos), y por defecto las variables **ganan** sobre los archivos JSON. Reglas clave:
- El separador jerárquico `:` se escribe como **doble guion bajo** `__` en variables de entorno (porque `:` no es válido en todos los sistemas). `ConnectionStrings:Default` se vuelve `ConnectionStrings__Default`; `Hecate:Jwt:Emisor` se vuelve `Hecate__Jwt__Emisor`.
- Las variables con prefijo `ASPNETCORE_` y `DOTNET_` configuran el host; `ASPNETCORE_ENVIRONMENT` (o `DOTNET_ENVIRONMENT`) define el ambiente (`Development`, `Staging`, `Production`), que decide qué `appsettings.{Ambiente}.json` se carga.
- Se integran con el [[Options pattern]] igual que cualquier otra fuente.

### Ejemplo
Arrancar Itzamna.Dionisio con distinta configuración:

```bash
 # bash: solo para este comando
ASPNETCORE_ENVIRONMENT=Staging dotnet run --project src/Itzamna.Dionisio.Api

 # para toda la sesión
export ConnectionStrings__Default="Server=localhost,1433;Database=Dionisio;Integrated Security=false;User Id=app;Password=..."
export Logging__LogLevel__Default=Debug

 # ver las que empiezan con ASPNETCORE_
env | grep '^ASPNETCORE_'
```

```powershell
 # PowerShell: sesión actual
$env:ASPNETCORE_ENVIRONMENT = "Staging"
dotnet run --project src/Itzamna.Dionisio.Api
Remove-Item Env:ASPNETCORE_ENVIRONMENT      # quitarla

Get-ChildItem Env: | Where-Object Name -like 'ASPNETCORE_*'
```

Leerla en código (casi siempre a través de `IConfiguration`, no directo):

```csharp
var builder = WebApplication.CreateBuilder(args);
// ConnectionStrings__Default en el entorno sobrescribe el appsettings.json
var cadena = builder.Configuration.GetConnectionString("Default")
             ?? throw new InvalidOperationException("Falta ConnectionStrings:Default");
```

En Azure App Service, la sección **Configuración > Variables de entorno** (app settings) llega a la app como variables de entorno; ahí también se usa `__` para la jerarquía en Linux.

**¿Son seguras para secretos?** Son **mejores que dejar el secreto en el código o en el repo**, pero **no son una bóveda**:
- Cualquier proceso con permisos del mismo usuario (o root) puede leerlas (`/proc/<pid>/environ` en Linux).
- Se filtran fácilmente: volcados de diagnóstico, logs que imprimen la configuración, páginas de error, `docker inspect`, procesos hijos que las heredan.
- No tienen rotación, auditoría ni control de acceso propios.
Lo recomendable: en desarrollo, [[Secretos con User Secrets]]; en Azure, **Azure Key Vault** con identidad administrada (y, si se usan app settings, referencias a Key Vault en lugar del valor). En pipelines, variables secretas o grupos de variables enlazados a Key Vault.

### Cuándo usarlo y cuándo no
- **Sí**: valores que cambian por ambiente (ambiente, URLs, niveles de log, banderas), configuración de contenedores y servicios.
- **No**: estructuras grandes de configuración (mejor un JSON por ambiente); secretos de alto valor sin bóveda.
- No dependas de variables definidas "a mano" en una máquina: documenta las necesarias (por ejemplo, un `.env.example` sin valores).

### Errores comunes
- **Usar `:` en lugar de `__`** en Linux o en App Service Linux: .NET no la mapea.
- **Esperar que una terminal o el IDE ya abiertos vean la variable nueva**: reinícialos.
- **Mayúsculas/minúsculas**: en Linux los nombres distinguen mayúsculas (`Path` ≠ `PATH`); en Windows no.
- **Imprimir toda la configuración** en el log de arranque: expones secretos.
- **Olvidar comillas** en valores con espacios o `;` (cadenas de conexión).

### Para profundizar
- [Configuración en ASP.NET Core (proveedor de variables de entorno)](https://learn.microsoft.com/aspnet/core/fundamentals/configuration/)
- [Usar varios entornos en ASP.NET Core](https://learn.microsoft.com/aspnet/core/fundamentals/environments)
- [The Twelve-Factor App: configuración](https://12factor.net/es/config)
- [Referencias de Key Vault en App Service](https://learn.microsoft.com/azure/app-service/app-service-key-vault-references)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Define `ASPNETCORE_ENVIRONMENT=Development` solo para un comando en bash.
> `ASPNETCORE_ENVIRONMENT=Development dotnet run`

> [!question]- Lo mismo para la sesión actual de PowerShell.
> `$env:ASPNETCORE_ENVIRONMENT = 'Development'`

> [!question]- Muestra todas las variables que empiezan con `DOTNET_` en bash y en PowerShell.
> bash: `env | grep '^DOTNET_'`. PowerShell: `Get-ChildItem Env:DOTNET_*`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirven las variables de entorno en una app?
> Para configuración que cambia por entorno (cadenas de conexión, URLs, banderas) sin recompilar ni versionar secretos. Es uno de los principios de las aplicaciones Twelve-Factor.

> [!question]- ¿Cómo se mapea `ConnectionStrings:Default` a variable de entorno en .NET?
> `ConnectionStrings__Default` (doble guion bajo en lugar de `:`), porque `:` no es válido en todos los sistemas.

> [!question]- ¿Por qué una variable que definiste en una terminal no la ve tu app lanzada desde otra?
> Las variables se heredan del proceso padre al crearse el hijo. En bash, `export X=1` solo afecta esa sesión y sus hijos; en Windows, `setx` afecta nuevas sesiones, no las abiertas.

> [!question]- ¿Las variables de entorno son un lugar seguro para secretos?
> Mejor que el código, pero cualquier proceso del mismo usuario o un volcado de diagnóstico puede leerlas. En la nube, prefiere Key Vault con identidad administrada.

## Práctica

- [[Lab 1 - Terminal, variables de entorno y procesos]]

