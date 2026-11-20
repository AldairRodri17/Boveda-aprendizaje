---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Sesiones

Parte de: [[02-Fundamentos/05 Autenticación y autorización/_Índice Autenticación y autorización|Autenticación y autorización]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **sesión del lado del servidor** es un registro que el servidor guarda (en memoria, Redis o una base de datos) con el estado de un usuario autenticado: quién es, cuándo inició sesión, desde qué dispositivo. Al navegador solo le entrega un **identificador de sesión** aleatorio en una cookie. En cada petición, el servidor busca ese id y recupera el estado.

Es el modelo opuesto al de un **token autocontenido** (JWT), donde el estado viaja firmado dentro del token y el servidor no guarda nada.

### Cómo funciona
```mermaid
sequenceDiagram
    participant N as Navegador
    participant S as Servidor
    participant R as Almacén (Redis / SQL)
    N->>S: POST /login
    S->>R: INSERT sesión (id aleatorio, usuario, expira)
    S-->>N: Set-Cookie: __Host-sid=<id>; HttpOnly; Secure; SameSite=Lax
    N->>S: GET /pedidos + Cookie: __Host-sid=<id>
    S->>R: SELECT sesión por id
    R-->>S: usuario 42, vigente
    S-->>N: 200
    N->>S: POST /logout
    S->>R: DELETE sesión
```

**Ventajas frente a un JWT autocontenido:**
- **Revocación inmediata**: borrar el registro invalida la sesión al instante (logout, cambio de contraseña, "cerrar sesión en todos los dispositivos", bloqueo por fraude). Con un JWT tienes que esperar a que expire o mantener una lista de revocados, que es justo una sesión.
- **Cookie pequeña**: solo el id, no todos los claims.
- **Visibilidad**: puedes listar sesiones activas por usuario.

**Costo**: necesitas un almacén **compartido** entre instancias (si escalas a 3 instancias, las 3 deben ver la misma sesión) y una consulta por petición (mitigable con caché).

**Requisitos del id de sesión:**
- Aleatorio criptográfico con suficiente entropía (128 bits o más), nunca secuencial ni derivado del usuario.
- En cookie `HttpOnly`, `Secure`, `SameSite`, idealmente con prefijo `__Host-` (ver [[Cookies - Secure y HttpOnly]]).
- Expiración por inactividad y expiración absoluta.

**Fijación de sesión.** El atacante consigue que la víctima use un id de sesión que él conoce (por ejemplo, plantándole una cookie desde un subdominio o pasando el id en la URL en sistemas viejos) y espera a que la víctima inicie sesión. Si el servidor **conserva** el mismo id tras el login, el atacante ya tiene una sesión autenticada. Se evita **generando un id nuevo al autenticarse** (y al elevar privilegios) y descartando el anterior.

### Ejemplo
Tabla y procedimientos para sesiones revocables en Itzamna.Dionisio (SQL Server), usados por un validador de cookie:

```sql
-- Sesiones/CrearSesion.sql (cuerpo del procedimiento dbo.Sesion_Crear)
INSERT INTO dbo.Sesion (Id, UsuarioId, CreadaUtc, UltimoUsoUtc, ExpiraUtc, Agente)
VALUES (@Id, @UsuarioId, SYSUTCDATETIME(), SYSUTCDATETIME(), @ExpiraUtc, @Agente);
```

```sql
-- Sesiones/SesionVigente.sql
SELECT CAST(CASE WHEN EXISTS (
    SELECT 1 FROM dbo.Sesion
    WHERE Id = @Id AND RevocadaUtc IS NULL AND ExpiraUtc > SYSUTCDATETIME()
) THEN 1 ELSE 0 END AS bit);
```

```csharp
// Al iniciar sesión: id NUEVO siempre (evita fijación)
var sid = Convert.ToHexString(RandomNumberGenerator.GetBytes(32));
await crearSesion.EjecutarAsync(sid, usuario.Id, DateTime.UtcNow.AddHours(8), agente, ct);

var principal = new ClaimsPrincipal(new ClaimsIdentity(
    [new Claim(ClaimTypes.NameIdentifier, usuario.Id.ToString()), new Claim("sid", sid)],
    CookieAuthenticationDefaults.AuthenticationScheme));
await HttpContext.SignInAsync(principal);
```

El evento `OnValidatePrincipal` de la cookie consulta `SesionVigente` y rechaza si fue revocada (ver [[Cookies de sesión y persistentes]]). Así obtienes lo mejor de ambos mundos: la cookie de ASP.NET Core protegida con Data Protection y la revocación de una sesión de servidor.

Nota: `AddSession()` de ASP.NET Core (estado de sesión) es otra cosa: guarda datos temporales de la app por navegador y **no** debe usarse como mecanismo de autenticación.

### Cuándo usarlo y cuándo no
- **Sesiones de servidor**: apps web propias, banca, sistemas internos, cualquier caso donde revocar al instante importe.
- **JWT autocontenidos**: APIs consumidas por muchos servicios o clientes, donde validar sin consultar a nadie escala mejor, con vidas cortas (ver [[Tokens de acceso]]).
- Un punto intermedio común: JWT de vida corta para APIs y sesión revocable en el IdP o en el BFF.

### Errores comunes
1. **No regenerar el id al iniciar sesión.** Fijación de sesión. Siempre un id nuevo.
2. **Ids predecibles** (autoincremento, GUID v1, hash del usuario). Usa `RandomNumberGenerator`.
3. **Sesiones en memoria con varias instancias.** El usuario cae en otra instancia y pierde la sesión. Usa Redis o SQL.
4. **Logout que solo borra la cookie.** Si alguien copió el id, sigue vigente. Borra o revoca en el servidor.
5. **Sin expiración absoluta.** Una sesión deslizante podría vivir para siempre; pon un máximo.

### Para profundizar
- [OWASP: Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [Microsoft Learn: gestión de estado y sesión en ASP.NET Core](https://learn.microsoft.com/aspnet/core/fundamentals/app-state)
- [Microsoft Learn: almacén de tickets y autenticación por cookie](https://learn.microsoft.com/aspnet/core/security/authentication/cookie)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una sesión del lado del servidor?
> El servidor guarda el estado (usuario, datos) en memoria, Redis o base de datos y le da al navegador solo un **identificador** aleatorio en una cookie.

> [!question]- ¿Qué ventaja tiene sobre un JWT autocontenido?
> Se puede **revocar** al instante (borrar la sesión) y la cookie es pequeña. A cambio, requiere almacenamiento compartido entre instancias.

> [!question]- ¿Qué es la fijación de sesión y cómo se evita?
> El atacante logra que la víctima use un id de sesión que él conoce y espera a que inicie sesión. Se evita **regenerando el id** al autenticarse.

