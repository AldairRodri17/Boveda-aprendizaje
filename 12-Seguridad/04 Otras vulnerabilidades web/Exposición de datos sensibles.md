---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Seguridad
tags: [seguridad]
---
# Exposición de datos sensibles

Parte de: [[12-Seguridad/04 Otras vulnerabilidades web/_Índice Otras vulnerabilidades web|Otras vulnerabilidades web]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La exposición de datos sensibles ocurre cuando información que debería estar protegida (datos personales, credenciales, tokens, datos financieros, detalles internos del sistema) llega a quien no debe verla. Rara vez es un ataque sofisticado: casi siempre es la app **entregando de más** por un diseño descuidado. En el OWASP Top 10 2025 se relaciona con *A04: Cryptographic Failures* (datos sin cifrar) y con *A01: Broken Access Control*; en México, además, tiene consecuencias legales por la ley de datos personales (ver [[Privacidad - ley mexicana de datos personales (LFPDPPP)]]).

### Cómo ocurre
Las formas más comunes, de la más a la menos frecuente en APIs .NET:

1. **Serializar la entidad completa**: el endpoint devuelve el objeto que vino de la base, con `HashContrasena`, `TokenRecuperacion`, `NotasInternas` o el `TenantId`. El front no los muestra, pero cualquiera los ve en la pestaña Network.
2. **Datos en la URL**: tokens, CURP o correos en la query string (`/descargar?token=...`). Las URLs quedan en logs de servidores y proxies, en el historial y en marcadores del navegador, y viajan a terceros en el header `Referer`.
3. **Errores detallados**: stack traces, cadenas de conexión o consultas SQL en respuestas de error (ver [[SQL injection - manejo de errores sin filtrar detalles]]).
4. **Logs**: registrar cuerpos de petición con contraseñas o tokens (ver [[Logs y datos sensibles]]).
5. **Transporte o almacenamiento sin cifrar**: HTTP en vez de HTTPS, respaldos sin cifrado, columnas sensibles en texto plano ([[Cifrado en reposo y en tránsito]]).
6. **Almacenamiento público**: un contenedor de Blob Storage con acceso anónimo o un respaldo `.bak` en una carpeta servida.
7. **Caché**: respuestas con datos personales cacheadas por un proxy o CDN porque faltó `Cache-Control: no-store`.

### Cómo se detecta
- **Revisa las respuestas reales**: abre DevTools o un proxy y compara qué campos llegan con los que el front usa. La diferencia es exposición.
- **Pruebas de contrato**: una prueba que deserializa la respuesta y falla si aparecen propiedades no esperadas.
- **Búsqueda en logs** de patrones como `password`, `Authorization`, `token=`.
- **Escaneo pasivo** con [[OWASP ZAP]]: detecta errores con información de depuración, falta de HTTPS y datos en URLs.
- **Clasifica tus datos**: no puedes proteger lo que no sabes que es sensible. Marca en el modelo qué columnas son datos personales o secretos.

### Cómo se previene
- **DTOs de salida explícitos** por caso de uso, que solo contienen los campos necesarios. En vertical slice, cada slice tiene su respuesta.
- **Proyección en SQL**: el procedimiento devuelve solo esas columnas; así ni siquiera viajan de la base a la API.
- **Nada sensible en URLs**: tokens en headers o en el cuerpo de un `POST`; para descargas, URLs firmadas de corta duración (por ejemplo [[Blob Storage - SAS (firmas de acceso compartido)]]) y `Referrer-Policy: strict-origin-when-cross-origin` o `no-referrer`.
- **Errores genéricos** con [[ProblemDetails]] y un `codigo` estable; el detalle va solo a los logs, con un id de correlación.
- **`Cache-Control: no-store`** en respuestas con datos personales.
- **Minimización**: si no necesitas un dato, no lo pidas ni lo guardes.

### Ejemplo
Antes, en Itzamna.Dionisio:

```csharp
// MAL: devuelve todo lo que trae la entidad
[HttpGet("{id:int}")]
public async Task<Usuario?> Obtener(int id) => await repo.ObtenerAsync(id);
```

Después, con DTO y procedimiento que proyecta solo lo necesario:

```sql
-- Sql/Usuarios/ObtenerPerfil.sql
CREATE OR ALTER PROCEDURE dbo.ObtenerPerfil @UsuarioId INT, @TenantId INT
AS
BEGIN
    SET NOCOUNT ON;
    SELECT u.Id, u.Nombre, u.Correo, u.FechaAlta
      FROM dbo.Usuario AS u
     WHERE u.Id = @UsuarioId AND u.TenantId = @TenantId;
END
```

```csharp
public sealed record PerfilRespuesta(int Id, string Nombre, string Correo, DateTime FechaAlta);

[HttpGet("perfil")]
public async Task<IActionResult> Perfil(CancellationToken ct)
{
    var (usuarioId, tenantId) = User.ObtenerIds();          // extensión propia: lee claims del token
    var perfil = await consulta.EjecutarAsync(usuarioId, tenantId, ct);
    Response.Headers.CacheControl = "no-store";
    return perfil is null ? NotFound() : Ok(perfil);
}
```

En el front, si una descarga necesita autorización, en lugar de `window.open('/api/reportes/5?token=...')` pide a la API una URL SAS de pocos minutos y ábrela, o descarga con `fetch` y el header `Authorization`, creando un `Blob` local.

### Cuándo usarlo y cuándo no
Los DTOs de salida son la regla en cualquier API pública o consumida por un navegador. Para servicios internos, la tentación de "devolver la entidad" es mayor, pero los servicios internos también se exponen con el tiempo; el costo de un DTO es mínimo frente al de una filtración.

### Errores comunes
- **Usar `[JsonIgnore]` en la entidad** como única defensa: funciona hasta que alguien agrega una columna nueva y olvida marcarla. El DTO hace que lo nuevo quede fuera por defecto.
- **Ocultar en el front lo que la API sí manda**.
- **Pensar que HTTPS resuelve las URLs**: cifra el tránsito, pero la URL igual queda en logs del servidor e historial.
- **Mensajes de error "amigables" para el desarrollador** que se quedan en producción.

### Para profundizar
- [OWASP API Security Top 10 2023 - API3 Broken Object Property Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa3-broken-object-property-level-authorization/)
- [OWASP - REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
- [MDN - Referrer-Policy](https://developer.mozilla.org/es/docs/Web/HTTP/Headers/Referrer-Policy)
- [CWE-200: Exposure of Sensitive Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/200.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Menciona formas comunes de exponer datos sensibles.
> Respuestas de API que devuelven la entidad completa (con hash de contraseña o datos internos), datos sin cifrar en tránsito, respaldos públicos, mensajes de error detallados, datos en URLs (quedan en logs e historial).

> [!question]- ¿Cómo evitas devolver campos de más en una API .NET?
> Con DTOs de respuesta explícitos que solo tienen los campos necesarios, en lugar de serializar la entidad de base de datos.

> [!question]- ¿Por qué no deben ir tokens o datos personales en la query string?
> Las URLs se guardan en logs de servidores y proxies, en el historial del navegador y viajan en el header `Referer`.

## Práctica

- [[Lab 8 - Control de acceso roto en OWASP Juice Shop local]]

