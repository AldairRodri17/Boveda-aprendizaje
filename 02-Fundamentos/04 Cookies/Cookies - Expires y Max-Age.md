---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
contenido: completo
---
# Cookies - Expires y Max-Age

Parte de: [[02-Fundamentos/04 Cookies/_Índice Cookies|Cookies]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
`Expires` y `Max-Age` controlan **cuánto vive** una cookie en el navegador. Sin ninguno de los dos, la cookie es de sesión del navegador y se borra al cerrarlo (ver [[Cookies de sesión y persistentes]]). Con alguno, es persistente hasta el momento indicado.

- **`Expires`**: una fecha y hora **absoluta** en formato HTTP (siempre en GMT): `Expires=Wed, 21 Oct 2026 07:28:00 GMT`.
- **`Max-Age`**: un número de **segundos** contados desde que el navegador recibe la cookie: `Max-Age=3600` es una hora.

### Cómo funciona
1. Al recibir el `Set-Cookie`, el navegador calcula el momento de expiración. Con `Max-Age` es "ahora + N segundos" según su propio reloj; con `Expires` es la fecha literal.
2. **Si vienen ambos, gana `Max-Age`** (lo dice la RFC 6265 y lo respetan los navegadores modernos). Se suele enviar los dos solo por compatibilidad con clientes muy viejos.
3. `Max-Age=0` o un valor negativo, o un `Expires` en el pasado, significa "bórrala ya".
4. Al llegar la expiración, la cookie deja de enviarse y se elimina de la jarra.

**Por qué preferir `Max-Age`.** `Expires` depende de que el reloj del cliente esté bien: si la computadora del usuario tiene la hora mal por dos horas, una cookie de "1 hora" calculada con `Expires` puede nacer ya vencida. `Max-Age` es relativo al momento en que se recibe y no requiere parsear fechas.

**Topes del navegador.** Los navegadores basados en Chromium limitan la vida máxima de una cookie a unos 400 días aunque pidas más, siguiendo el borrador RFC 6265bis; otros navegadores aplican sus propias reglas, como Safari, que acorta la vida de cookies creadas por JavaScript en ciertos contextos. No diseñes nada que dependa de cookies de varios años.

### Ejemplo
Cálculos típicos (útiles para los ejercicios):

| Necesidad | Atributo |
|---|---|
| 30 minutos | `Max-Age=1800` |
| 1 hora | `Max-Age=3600` |
| 7 días | `Max-Age=604800` (7 × 24 × 3600) |
| 30 días | `Max-Age=2592000` |
| 1 año | `Max-Age=31536000` |
| Borrar | `Max-Age=0` |

Si una cookie con `Max-Age=1800` se recibe a las 10:05, expira a las 10:35 de ese día: 1,800 segundos son 30 minutos.

En el controller de autenticación de Itzamna.Dionisio, una cookie de preferencia de 7 días y el borrado de la cookie `tema`:

```csharp
// 7 días: genera "max-age=604800" y también "expires=..." por compatibilidad
Response.Cookies.Append("tema", "oscuro", new CookieOptions
{
    MaxAge = TimeSpan.FromDays(7),
    Path = "/",
    Secure = true,
    SameSite = SameSiteMode.Lax
});

// Borrar ya: Delete envía la cookie vacía con una fecha en el pasado
Response.Cookies.Delete("tema", new CookieOptions { Path = "/" });
```

Para la **cookie de autenticación**, ASP.NET Core separa dos conceptos que conviene no confundir:
- `ExpireTimeSpan` en las opciones de `AddCookie`: cuánto vale el **ticket** de autenticación dentro de la cookie (el servidor lo rechaza al vencer, aunque el navegador aún la tenga).
- `IsPersistent` en `AuthenticationProperties`: si la cookie se emite con `Expires` (persistente, "Recordarme") o sin él (de sesión).

```csharp
await HttpContext.SignInAsync(principal, new AuthenticationProperties
{
    IsPersistent = solicitud.Recordarme,          // sin Recordarme: cookie de sesión
    ExpiresUtc = DateTimeOffset.UtcNow.AddDays(14) // vigencia del ticket
});
```

La lección: el vencimiento que **importa para seguridad** es el que valida el servidor (dentro del valor firmado), no el que aplica el navegador, porque el cliente puede conservar una cookie todo lo que quiera.

### Cuándo usarlo y cuándo no
- **Usa `Max-Age`** para cualquier cookie persistente: preferencias, "Recordarme", consentimiento de cookies.
- **No pongas caducidad** a cookies que deben morir con el navegador: correlación de login, antiforgery por sesión, datos de un asistente.
- **No confíes solo en la expiración del navegador** para cerrar sesiones: valida la vigencia en el servidor.

### Errores comunes
1. **Usar hora local en `Expires`.** El formato exige GMT; si generas la fecha a mano con hora de México, la cookie vence seis horas antes o después. Deja que el framework lo genere o usa `Max-Age`.
2. **Confundir segundos con milisegundos.** `Max-Age=604800000` no es una semana sino unos 19 años (y el navegador lo recorta). `Max-Age` siempre va en segundos.
3. **Creer que al vencer la cookie la sesión se invalida en el servidor.** Si alguien copió el valor, puede seguir usándolo mientras el servidor lo acepte.
4. **Borrar sin el mismo `Path`/`Domain`.** Ver [[Set-Cookie y el header Cookie]].
5. **Cookies "eternas".** Además del tope de unos 400 días en Chromium, una sesión muy larga sin revalidación es un riesgo; combina con expiración deslizante y revocación.

### Para profundizar
- [MDN: Set-Cookie, atributos Expires y Max-Age](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie)
- [RFC 6265, sección 5.2.1 y 5.2.2](https://www.rfc-editor.org/rfc/rfc6265#section-5.2.1)
- [Microsoft Learn: autenticación con cookies sin Identity](https://learn.microsoft.com/aspnet/core/security/authentication/cookie)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe el atributo para que una cookie dure 7 días.
> `Max-Age=604800` (7 × 24 × 60 × 60).

> [!question]- Una cookie con `Max-Age=1800` se recibió a las 10:05. ¿Cuándo expira?
> A las **10:35** (1800 s = 30 min).

> [!question]- ¿Qué atributo pones para borrar ya la cookie `tema`?
> `Set-Cookie: tema=; Max-Age=0; Path=/` (con los mismos `Domain`/`Path` con que se creó).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre `Expires` y `Max-Age`?
> `Expires` es una fecha absoluta (`Expires=Wed, 21 Oct 2026 07:28:00 GMT`); `Max-Age` son segundos desde ahora. Si vienen ambos, gana `Max-Age`.

> [!question]- ¿Qué pasa si no se pone ninguno?
> Es una **cookie de sesión**: el navegador la borra al cerrar la sesión del navegador (aunque la función de "restaurar pestañas" puede conservarla).

> [!question]- ¿Por qué preferir `Max-Age`?
> No depende del reloj del cliente ni de formatos de fecha: es relativo al momento de recibirla.

## Práctica

- [[Lab 1 - Cookies y CORS en el navegador con DevTools]]

