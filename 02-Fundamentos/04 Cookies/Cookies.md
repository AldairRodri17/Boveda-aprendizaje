---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Cookies

Parte de: [[02-Fundamentos/04 Cookies/_Índice Cookies|Cookies]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **cookie** es un par nombre-valor pequeño que un servidor le pide al navegador guardar, junto con unas reglas (atributos) que indican a qué peticiones debe adjuntarse y por cuánto tiempo. El navegador la reenvía sola en cada petición que cumpla esas reglas. Existen porque HTTP es un protocolo **sin estado**: cada petición llega sola, sin memoria de las anteriores, y la cookie es la forma estándar de "recordar" algo entre peticiones (quién eres, qué idioma elegiste, qué hay en tu carrito).

Sus tres usos clásicos son:
- **Gestión de sesión**: login, carrito, estado de un asistente de varios pasos.
- **Preferencias**: tema oscuro, idioma, zona horaria.
- **Seguimiento**: analítica y publicidad, que es justo el uso que los navegadores restringen cada vez más.

### Cómo funciona
El ciclo completo usa dos headers que se explican en [[Set-Cookie y el header Cookie]]:

```mermaid
sequenceDiagram
    participant N as Navegador
    participant S as Servidor (api.ejemplo.com)
    N->>S: POST /login (usuario y contraseña)
    S-->>N: 200 OK + Set-Cookie: sid=7f3a...; Secure; HttpOnly; SameSite=Lax
    Note over N: Guarda la cookie con sus atributos
    N->>S: GET /pedidos + Cookie: sid=7f3a...
    S-->>N: 200 OK (el servidor reconoce la sesión)
```

1. El servidor responde con `Set-Cookie`, que trae el valor y los atributos.
2. El navegador la guarda en su "jarra" de cookies (cookie jar), asociada al host o dominio.
3. En cada petición siguiente, el navegador busca qué cookies aplican (por dominio, ruta, `Secure`, `SameSite`, vencimiento) y las manda en un único header `Cookie`, **solo con nombre y valor**.

Los atributos son el corazón del tema y cada uno tiene su nota:
- Alcance: [[Cookies - Domain y Path]].
- Duración: [[Cookies - Expires y Max-Age]] y [[Cookies de sesión y persistentes]].
- Seguridad: [[Cookies - Secure y HttpOnly]] y [[Cookies - SameSite]].
- Contexto entre sitios: [[Mismo origen, mismo sitio y mismo dominio]], [[Cookies de primera y tercera parte]] y [[Restricciones a cookies de terceros en navegadores]].

**Límites prácticos.** La especificación (RFC 6265) pide que los navegadores soporten *al menos* unos 4,096 bytes por cookie (nombre, valor y atributos) y al menos 50 cookies por dominio. En la práctica, piensa en "unos 4 KB por cookie y pocas decenas por dominio". Lo más importante: **todas las cookies que coinciden viajan en cada petición**, incluidas las de imágenes, CSS y llamadas a API. Una cookie de 3 KB en un sitio con 80 peticiones por página son 240 KB extra de subida por cada carga. Por eso no se usan como almacén de datos.

### Ejemplo
En Itzamna.Dionisio, si quieres guardar la preferencia de tema del usuario y su sesión, la respuesta del login de un sitio con cookies se vería así:

```http
HTTP/1.1 200 OK
Set-Cookie: __Host-sid=CfDJ8N...cifrado...; Path=/; Secure; HttpOnly; SameSite=Lax
Set-Cookie: tema=oscuro; Path=/; Max-Age=31536000; SameSite=Lax
```

La cookie de sesión lleva un valor **cifrado y firmado** (en ASP.NET Core lo produce Data Protection; ver [[Cookies en ASP.NET Core (autenticación y Data Protection)]]); la de tema no es sensible y puede leerse desde JavaScript. Lo que **no** harías es esto:

```http
Set-Cookie: usuario={"id":42,"rol":"Admin","saldo":1500}
```

Cualquiera puede abrir DevTools, cambiar `"rol":"Admin"` y reenviarla. El servidor nunca debe confiar en el contenido de una cookie que no esté firmada, y si contiene datos personales, además debe ir cifrada o, mejor, ser solo un identificador opaco que apunte a datos guardados en el servidor (ver [[Sesiones]]).

### Cuándo usarlo y cuándo no
Úsalas para:
- La sesión de una aplicación web propia (con `HttpOnly`, `Secure` y `SameSite`).
- Preferencias pequeñas que el servidor necesita conocer en cada petición (idioma para renderizar).
- Tokens antiforgery y cookies de correlación de flujos de login (OIDC).

No las uses para:
- Guardar datos voluminosos o que solo necesita el navegador: para eso está Web Storage o IndexedDB (ver [[Cookies vs localStorage vs sessionStorage]]).
- Autenticar clientes que no son navegador (apps móviles, servicios, CLIs): ahí van tokens Bearer (ver [[Cookies vs tokens]]).
- Rastreo entre sitios: cada vez funciona menos y tiene implicaciones legales de privacidad.

### Errores comunes
1. **Guardar datos sensibles en claro.** El usuario los ve y los modifica, y un script los lee si la cookie no es `HttpOnly`. Guarda un id de sesión o un valor protegido criptográficamente.
2. **Inflar las cookies.** Pasa cuando se usan como caché. Todas las peticiones se vuelven más pesadas y se pueden rebasar límites de tamaño de headers en el servidor o el proxy (error `400 Request Header Too Large`).
3. **Olvidar los atributos de seguridad.** Una cookie de sesión sin `Secure`, `HttpOnly` y `SameSite` queda expuesta a robo por red, por XSS y a CSRF (ver [[Cookies y CSRF]]).
4. **Confiar en que el valor no cambió.** El cliente controla su jarra de cookies; valida siempre en el servidor.
5. **Pensar que CORS o `Path` aíslan cookies.** No lo hacen; el aislamiento real lo dan el origen, `HttpOnly` y los prefijos `__Host-`.

### Para profundizar
- [MDN: Uso de cookies HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)
- [RFC 6265: HTTP State Management Mechanism](https://www.rfc-editor.org/rfc/rfc6265)
- [Borrador RFC 6265bis (versión en revisión)](https://datatracker.ietf.org/doc/draft-ietf-httpbis-rfc6265bis/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una cookie y por qué existe?
> Un dato pequeño que el servidor pide guardar al navegador (`Set-Cookie`) y que este reenvía automáticamente en las peticiones siguientes al mismo sitio (`Cookie`). Existe porque HTTP no tiene estado: permite sesiones, preferencias y seguimiento.

> [!question]- ¿Qué límites prácticos tienen las cookies?
> Alrededor de **4 KB** por cookie (nombre + valor + atributos) y un número limitado por dominio; todas viajan en **cada** petición que coincida, así que inflarlas hace más lentas todas las peticiones.

> [!question]- Error común: guardar datos sensibles en claro dentro de una cookie. ¿Por qué es un problema?
> El usuario y cualquier script (si no es `HttpOnly`) pueden leerla y modificarla. Guarda un identificador de sesión o un valor firmado y cifrado (Data Protection en ASP.NET Core).

## Práctica

- [[Lab 1 - Cookies y CORS en el navegador con DevTools]]

