---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Seguridad
tags: [seguridad]
contenido: completo
---
# Logs y datos sensibles

Parte de: [[12-Seguridad/01 Fundamentos de seguridad/_Índice Fundamentos de seguridad|Fundamentos de seguridad]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Los logs son indispensables para depurar y para seguridad, pero también son una **copia de datos que suele protegerse peor** que la base: se mandan a Application Insights, a Sentry, a archivos, a la consola del pipeline, y los lee mucha más gente. Este tema trata de registrar lo útil **sin** convertir los logs en una fuga.

Nunca deben aparecer en logs: contraseñas, tokens (JWT, refresh tokens, códigos de autorización de PKCE), llaves de API, cadenas de conexión, cookies de sesión, números de tarjeta y CVV, y datos personales sensibles (salud, datos biométricos, CURP completa) salvo que exista una razón documentada y protección equivalente.

### Cómo funciona
Hay dos frentes:

1. **No escribir datos sensibles desde el origen**: registrar identificadores en lugar de objetos completos, usar plantillas estructuradas con campos elegidos y configurar el logging HTTP para excluir headers y cuerpos.
2. **Limpiar en la salida**: filtros o *redactores* que enmascaran campos antes de mandar el evento al destino. En Sentry se hace con el callback `BeforeSend`, que recibe el evento y puede modificarlo o descartarlo.

Lo que **sí** conviene registrar para seguridad (y lo que pide la categoría de fallas de registro y monitoreo del [[OWASP Top 10]]):
- Inicios de sesión exitosos y fallidos, bloqueos de cuenta, cambios de contraseña o de MFA.
- Fallas de autorización (403), sobre todo repetidas.
- Errores de validación en volumen anormal.
- Acciones administrativas y cambios de permisos.
- Para cada evento: quién (id de usuario, no su correo si no hace falta), cuándo, desde dónde (IP), qué recurso, resultado y un `traceId` para correlacionar.

### Ejemplo
Registro estructurado sin objetos completos en una API .NET:

```csharp
// Mal: serializa todo, incluido el password y el token
// logger.LogInformation("Login {@Request}", request);

// Bien: solo lo necesario
logger.LogWarning("Login fallido para usuario {UsuarioId} desde {Ip}. Traza {TraceId}",
    usuarioId, httpContext.Connection.RemoteIpAddress, Activity.Current?.TraceId);
```

Logging HTTP de ASP.NET Core con campos mínimos:

```csharp
builder.Services.AddHttpLogging(o =>
{
    o.LoggingFields = HttpLoggingFields.RequestMethod | HttpLoggingFields.RequestPath
                    | HttpLoggingFields.ResponseStatusCode | HttpLoggingFields.Duration;
    // Sin headers ni cuerpos: Authorization y cookies no salen
});
```

Limpieza antes de enviar a Sentry:

```csharp
builder.WebHost.UseSentry(o =>
{
    o.SendDefaultPii = false; // no mandar IP, usuario ni cookies automáticamente
    o.SetBeforeSend((evento, hint) =>
    {
        evento.Request.Headers.Remove("Authorization");
        evento.Request.Cookies = null;
        if (evento.Request.Data is string cuerpo && cuerpo.Contains("password", StringComparison.OrdinalIgnoreCase))
            evento.Request.Data = "[eliminado]";
        return evento; // devolver null descarta el evento
    });
});
```

En el front (Itzamna.Clio) aplica lo mismo: el SDK de Sentry para React también tiene `beforeSend`, y `console.log(token)` en producción es un log más.

### Cuándo usarlo y cuándo no
Registra eventos de seguridad siempre, con retención definida y acceso restringido. Evita el registro de cuerpos completos salvo en depuración puntual, en un ambiente controlado y por tiempo limitado. Si de verdad necesitas correlacionar con un dato sensible, guarda un hash o un valor enmascarado (últimos 4 dígitos), no el dato.

### Errores comunes
- **Registrar el objeto de la petición completo "para depurar".** Arrastra contraseñas, tokens, datos personales y a veces archivos en base64. Esos datos quedan replicados en herramientas de terceros, con otra retención y más lectores; una filtración del sistema de logs se convierte en una filtración de datos personales con obligaciones legales. Ver [[Privacidad y protección de datos (ley mexicana y GDPR)]].
- **Excepciones con datos dentro**: mensajes como "no se encontró el cliente con RFC X" o cadenas de conexión en el texto del error.
- **Logs sin `traceId`**: no puedes reconstruir un incidente.
- **Inyección en logs**: escribir texto del usuario tal cual permite falsificar líneas con saltos de línea. Los logs estructurados lo reducen.
- **Nadie revisa**: registrar sin alertas es solo almacenamiento.

### Para profundizar
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [Logging HTTP en ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/http-logging/)
- [Sentry .NET: filtrado de eventos](https://docs.sentry.io/platforms/dotnet/configuration/filtering/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué datos nunca deben aparecer en logs?
> Contraseñas, tokens y cookies de sesión, encabezados `Authorization`, números de tarjeta, datos personales sensibles (salud, CURP completo) y secretos de configuración.

> [!question]- ¿Cómo limpias eventos antes de mandarlos a Sentry?
> Con el callback `BeforeSend` (en .NET `SetBeforeSend`) para quitar o enmascarar headers, cuerpos y datos de usuario; además, configuración del lado de Sentry para depurar campos y no enviar PII por defecto (`SendDefaultPii = false`).

> [!question]- ¿Qué sí conviene registrar para seguridad?
> Inicios de sesión exitosos y fallidos, cambios de permisos, accesos denegados, errores de validación de tokens, con usuario, fecha, IP y un id de correlación, sin datos sensibles.

> [!question]- Error común: registrar el objeto de la petición completo "para depurar". ¿Qué riesgo trae?
> Termina en el log la contraseña o el token que venía en el cuerpo o en los headers, y cualquiera con acceso a los logs (o a la herramienta externa) los ve.

## Práctica

- [[Lab 1 - Sacar secretos del código en una API .NET]]

