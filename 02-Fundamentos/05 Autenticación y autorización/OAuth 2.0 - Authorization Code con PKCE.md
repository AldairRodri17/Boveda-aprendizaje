---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# OAuth 2.0 - Authorization Code con PKCE

Parte de: [[02-Fundamentos/05 Autenticación y autorización/_Índice Autenticación y autorización|Autenticación y autorización]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Authorization Code** es el flujo de OAuth 2.0 en el que la app redirige al usuario al servidor de autorización, el usuario inicia sesión y consiente, y la app recibe un **código de un solo uso** que luego **canjea** por los tokens en una llamada directa al endpoint de token. **PKCE** (Proof Key for Code Exchange, RFC 7636, se pronuncia "pixi") le agrega una prueba criptográfica para que **solo quien inició el flujo** pueda canjear el código.

Es el flujo recomendado para **toda** app con usuario: web con backend, SPA, móvil y escritorio.

### Cómo funciona
```mermaid
sequenceDiagram
    participant U as Usuario
    participant C as Cliente (Clio)
    participant AS as Servidor de autorización (Hecate)
    C->>C: code_verifier aleatorio; code_challenge = BASE64URL(SHA256(verifier)); state
    C->>U: 302 a /authorize?response_type=code&client_id&redirect_uri&scope&state&code_challenge&code_challenge_method=S256
    U->>AS: Inicia sesión (MFA) y consiente
    AS-->>U: 302 a redirect_uri?code=SplxlOBeZQ&state=...
    U->>C: GET /callback?code&state
    C->>C: Verifica state
    C->>AS: POST /token grant_type=authorization_code&code&redirect_uri&client_id&code_verifier
    AS->>AS: SHA256(code_verifier) == code_challenge guardado?
    AS-->>C: access_token, id_token (OIDC), refresh_token
```

**El flujo base:**
1. La app redirige al endpoint de autorización con `response_type=code`, `client_id`, `redirect_uri`, `scope` y `state`.
2. El usuario se autentica y consiente **en el sitio del servidor de autorización** (la app nunca ve su contraseña).
3. El servidor redirige a `redirect_uri` con `code` y el mismo `state`.
4. La app verifica `state` y **canjea** el código en el endpoint de token (canal directo, no el navegador del usuario).

**Lo que agrega PKCE:**
- Antes de redirigir, la app genera un **`code_verifier`**: cadena aleatoria de 43 a 128 caracteres del conjunto `[A-Z a-z 0-9 - . _ ~]`.
- Calcula **`code_challenge = BASE64URL(SHA256(ASCII(code_verifier)))`** sin relleno `=` y lo manda con `code_challenge_method=S256`.
- El servidor guarda el challenge junto al código.
- Al canjear, la app manda el **`code_verifier`** original; el servidor calcula el hash y compara.

**Ataque que evita**: la **interceptación del código**. En móvil, otra app maliciosa puede registrar el mismo esquema de URL personalizado y recibir la redirección; en web, el código puede filtrarse en logs, historial o `Referer`. Sin PKCE, quien tenga el código lo canjea. Con PKCE, el código solo no sirve: falta el verifier, que nunca salió de la app. También protege contra la inyección de códigos (un atacante que mete su código en tu sesión).

**¿Solo para apps públicas?** No. Nació para apps móviles, pero la BCP (RFC 9700) lo recomienda para todos los clientes y OAuth 2.1 lo exige también para clientes confidenciales que tienen secreto: el secreto autentica a la app, PKCE vincula **ese** código con **esa** transacción.

**¿Por qué `S256` y no `plain`?** Con `plain`, el challenge **es** el verifier. Quien vea la petición inicial (por ejemplo en logs del navegador o de un proxy) ya tiene el secreto. Con `S256` solo se expone un hash del que no se puede obtener el verifier. `plain` solo existe para clientes que no pueden calcular SHA-256, que hoy prácticamente no existen.

### Ejemplo
Cálculo verificado del ejemplo del RFC 7636 (apéndice B), en Python:

```python
import base64, hashlib, secrets

verifier = "dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk"
challenge = base64.urlsafe_b64encode(hashlib.sha256(verifier.encode("ascii")).digest()).rstrip(b"=").decode()
print(challenge)  # E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM

# Generar un verifier válido: 32 bytes aleatorios → 43 caracteres Base64URL sin relleno
nuevo = base64.urlsafe_b64encode(secrets.token_bytes(32)).rstrip(b"=").decode()
print(len(nuevo))  # 43
```

Lo mismo en TypeScript para el navegador (Itzamna.Clio), con Web Crypto:

```ts
function base64Url(bytes: Uint8Array): string {
  return btoa(String.fromCharCode(...bytes)).replace(/\+/g, "-").replace(/\//g, "_").replace(/=+$/, "");
}

export async function crearPkce() {
  const verifier = base64Url(crypto.getRandomValues(new Uint8Array(32)));
  const hash = await crypto.subtle.digest("SHA-256", new TextEncoder().encode(verifier));
  return { verifier, challenge: base64Url(new Uint8Array(hash)) };
}
```

Canje del código (lo que manda la app al endpoint de token):

```http
POST /connect/token HTTP/1.1
Host: id.xolotl.mx
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&code=SplxlOBeZQQYbYS6WxSbIA
&redirect_uri=https%3A%2F%2Fapp.itzamna.mx%2Fcallback
&client_id=itzamna-clio
&code_verifier=dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```

Un cliente confidencial agrega además su autenticación (`client_secret` o `client_assertion`). El `redirect_uri` debe ser idéntico al de la petición inicial. En ASP.NET Core, `AddOpenIdConnect` con `ResponseType = "code"` ya genera PKCE (`UsePkce = true` por defecto).

### Cuándo usarlo y cuándo no
- **Siempre** que haya un usuario y un navegador disponible: web, SPA, móvil (con el navegador del sistema, no un WebView embebido), escritorio (redirección a `localhost`).
- **No** para servicio a servicio (Client Credentials) ni para dispositivos sin navegador (Device Grant).

### Errores comunes
1. **No validar `state`** al volver. CSRF en el callback (ver [[OAuth 2.0]]).
2. **Usar `plain`** o reutilizar el mismo verifier. Genera uno nuevo por intento y usa `S256`.
3. **Guardar el verifier en `localStorage`** compartido entre pestañas. Usa `sessionStorage` o memoria de la transacción.
4. **Redirect URI distinta** en autorización y canje. El servidor rechaza con `invalid_grant`.
5. **WebViews embebidos en móvil.** La app podría leer la contraseña; usa el navegador del sistema (ASWebAuthenticationSession, Custom Tabs).

### Para profundizar
- [RFC 7636: Proof Key for Code Exchange](https://www.rfc-editor.org/rfc/rfc7636)
- [RFC 6749, sección 4.1 (Authorization Code Grant)](https://www.rfc-editor.org/rfc/rfc6749#section-4.1)
- [RFC 9700: Best Current Practice for OAuth 2.0 Security](https://www.rfc-editor.org/rfc/rfc9700)
- [oauth.net: PKCE](https://oauth.net/2/pkce/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Calcula el `code_challenge` (S256) para el verifier `dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk` (ejemplo del RFC 7636).
> `E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM`.
> ```python
> import hashlib, base64
> v = b'dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk'
> print(base64.urlsafe_b64encode(hashlib.sha256(v).digest()).rstrip(b'=').decode())
> ```

> [!question]- Genera en Python un `code_verifier` válido.
> ```python
> import secrets
> verifier = secrets.token_urlsafe(64)[:128]  # 43 a 128 caracteres del alfabeto permitido
> ```

> [!question]- ¿Qué parámetros lleva la petición de canje del code?
> `grant_type=authorization_code`, `code`, `redirect_uri` (el mismo del inicio), `client_id`, `code_verifier` y, si es cliente confidencial, su autenticación (`client_secret` o equivalente).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo funciona el flujo Authorization Code?
> 1. La app redirige al servidor de autorización con `response_type=code`, `client_id`, `redirect_uri`, `scope` y `state`. 2. El usuario se autentica y consiente. 3. Regresa a `redirect_uri` con `code`. 4. La app canjea el `code` en el endpoint de token y recibe los tokens.

> [!question]- ¿Qué agrega PKCE y qué ataque evita?
> La app genera un `code_verifier` aleatorio y envía `code_challenge = BASE64URL(SHA256(verifier))` al inicio; al canjear el code envía el `code_verifier`. Si alguien intercepta el code, no puede canjearlo sin el verifier.

> [!question]- ¿PKCE es solo para apps públicas?
> No. Se recomienda para **todos** los clientes, también los confidenciales con secreto (lo exige OAuth 2.1).

> [!question]- ¿Por qué `code_challenge_method` debe ser `S256` y no `plain`?
> Con `plain` el challenge es igual al verifier; quien vea la petición inicial ya tiene el secreto. `S256` solo expone el hash.

## Práctica

- [[Lab 2 - Authorization Code con PKCE contra un proveedor de prueba]]

