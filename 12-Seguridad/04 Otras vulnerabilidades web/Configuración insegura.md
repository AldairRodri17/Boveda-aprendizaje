---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Seguridad
tags: [seguridad]
---
# Configuración insegura

Parte de: [[12-Seguridad/04 Otras vulnerabilidades web/_Índice Otras vulnerabilidades web|Otras vulnerabilidades web]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La configuración insegura (*Security Misconfiguration*, A02 en el OWASP Top 10 2025) es cuando el código puede estar bien escrito, pero el entorno donde corre lo deja expuesto: opciones de depuración encendidas en producción, cuentas por defecto, servicios abiertos a internet, permisos amplios o headers de seguridad ausentes. Es de las fallas más comunes porque depende de muchas personas, archivos y entornos, y porque "funciona" igual con o sin el control.

### Cómo ocurre
La configuración vive en muchas capas, y cada una tiene sus clásicos:

| Capa | Ejemplos de configuración insegura |
|---|---|
| ASP.NET Core | Página de excepciones para desarrollador en producción, Swagger/Scalar público sin intención, `ASPNETCORE_ENVIRONMENT=Development` en un servidor, errores con stack trace en ProblemDetails |
| Navegador | Faltan `Strict-Transport-Security`, `Content-Security-Policy`, `X-Content-Type-Options`, `Referrer-Policy`, `frame-ancestors`; cookies sin `Secure` o `HttpOnly` |
| Red y servicios | Puerto 1433 de SQL Server abierto a internet, Redis sin contraseña, listado de directorios activo |
| Nube | Contenedores de Blob Storage con acceso público, reglas de firewall `0.0.0.0/0`, identidades con rol Owner |
| Datos | Cuenta `sa` usada por la app, usuarios de prueba con contraseñas conocidas, respaldos en carpetas públicas |

La causa raíz casi siempre es una de tres: valores por defecto pensados para desarrollo, cambios manuales que nadie documentó (deriva de configuración) o un "lo abro un momento para probar".

### Cómo se detecta
- **Escaneo pasivo** con [[OWASP ZAP]]: headers faltantes, cookies sin atributos, divulgación de versiones del servidor.
- **Revisión de infraestructura como código** (Bicep, Terraform) con analizadores que marcan almacenamiento público o reglas de red abiertas.
- **Microsoft Defender for Cloud** y sus recomendaciones sobre la suscripción de Azure.
- **Checklist manual** antes de cada salida a producción, idealmente convertido en pruebas automáticas.

### Cómo se previene
- **Configuración por entorno versionada**: `appsettings.Production.json` sin secretos, secretos en Key Vault ([[Manejo de secretos]]), y nada de condicionales que "abren todo" en desarrollo y se cuelan a producción.
- **Valores seguros por defecto**: lo restrictivo es lo normal y abrir requiere una decisión explícita.
- **Mínimo privilegio** en identidades, cuentas de base de datos y reglas de red ([[Principio de mínimo privilegio]]).
- **Headers de seguridad** aplicados en un solo lugar (middleware o el proxy de entrada).
- **Automatizar la verificación** en el pipeline para que la configuración insegura no regrese.

### Ejemplo
Arranque de Itzamna.Dionisio que separa lo de desarrollo y agrega headers básicos:

```csharp
var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();                 // documento OpenAPI solo en desarrollo
}
else
{
    app.UseExceptionHandler();        // ProblemDetails genérico, sin stack trace
    app.UseHsts();                    // Strict-Transport-Security
}

app.UseHttpsRedirection();

app.Use(async (ctx, next) =>
{
    ctx.Response.OnStarting(() =>
    {
        var h = ctx.Response.Headers;
        h["X-Content-Type-Options"] = "nosniff";
        h["Referrer-Policy"] = "strict-origin-when-cross-origin";
        // Para una API que solo devuelve JSON, una CSP mínima y restrictiva
        h["Content-Security-Policy"] = "default-src 'none'; frame-ancestors 'none'";
        h.Remove("Server");
        return Task.CompletedTask;
    });
    await next();
});
```

Notas:

- Para el front (Itzamna.Clio servido como estático), la CSP será distinta y más elaborada: ver [[Content Security Policy]].
- Quitar el header `Server` de Kestrel también se hace con `builder.WebHost.ConfigureKestrel(o => o.AddServerHeader = false)`; esconder la versión no es una defensa real, pero evita dar pistas gratis.
- `UseHsts` no se activa en desarrollo para no "pegar" HSTS a `localhost` en tu navegador.
- `ProblemDetails` con tu campo `codigo` debe llevar un mensaje útil para el cliente, nunca la excepción interna ([[ProblemDetails]]).

Para evitar que vuelva: un paso en Azure Pipelines que corre el baseline de ZAP contra el entorno de pruebas después de desplegar, y que falla si aparecen reglas marcadas como `FAIL` (ver [[Pruebas de seguridad en el pipeline]]).

### Cuándo usarlo y cuándo no
Los headers de seguridad básicos van en cualquier aplicación web. Algunos dependen del caso: HSTS con `preload` es un compromiso difícil de deshacer (todos tus subdominios deben soportar HTTPS); una CSP estricta requiere probar el front completo. Introdúcelos con modo de reporte primero cuando el riesgo de romper algo sea alto.

### Errores comunes
- **Arreglar en el portal y no en el código**: la siguiente vez que se despliega desde la plantilla, el problema regresa.
- **Swagger público "porque es útil"**: si no lo necesitan externos, publícalo solo en entornos internos o protégelo con autenticación.
- **Copiar headers de internet sin entenderlos**: `X-XSS-Protection` ya no se recomienda y una CSP mal hecha rompe la app.
- **Usar la misma cuenta y contraseña en todos los entornos**: un entorno de pruebas comprometido abre producción.

### Para profundizar
- [OWASP Top 10 2025 - A02 Security Misconfiguration](https://owasp.org/Top10/2025/)
- [OWASP - HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)
- [Microsoft Learn - Aplicar HTTPS en ASP.NET Core (HSTS)](https://learn.microsoft.com/aspnet/core/security/enforcing-ssl)
- [MDN - Guía de seguridad web](https://developer.mozilla.org/es/docs/Web/Security)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Ejemplos de configuración insegura.
> Página de excepciones de desarrollador en producción, Swagger público sin intención, cuentas por defecto, CORS abierto, falta de headers de seguridad, directorios listables, puertos de bases de datos expuestos, buckets o contenedores públicos.

> [!question]- ¿Qué headers de seguridad básicos agregarías a una app web?
> `Strict-Transport-Security`, `Content-Security-Policy`, `X-Content-Type-Options: nosniff`, `Referrer-Policy` y `frame-ancestors` (o `X-Frame-Options`).

> [!question]- ¿Cómo evitas que la configuración insegura vuelva?
> Infraestructura como código revisada, configuración por entorno versionada, checklists de despliegue y escaneos automáticos (por ejemplo, el baseline de [[OWASP ZAP]] en el pipeline).

## Práctica

- [[Lab 10 - Escaneo pasivo con OWASP ZAP contra Juice Shop local]]

