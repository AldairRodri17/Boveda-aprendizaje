---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: Keycloak en Docker (local y gratuito), navegador, Python 3 y curl
tags: [fundamentos, laboratorio]
contenido: completo
---
# Lab 2 - Authorization Code con PKCE contra un proveedor de prueba

Parte de: [[02-Fundamentos/05 Autenticación y autorización/_Índice Autenticación y autorización|Autenticación y autorización]]

**Cubre:** [[OAuth 2.0]] · [[OAuth 2.0 - Authorization Code con PKCE]] · [[OAuth 2.0 - roles y scopes]] · [[OpenID Connect]] · [[Refresh tokens]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Recorrer a mano, paso por paso, el flujo Authorization Code con PKCE y OpenID Connect: armar la URL de autorización, recibir el `code`, canjearlo y revisar los tokens.

## Requisitos
- Docker Desktop. Levanta Keycloak en modo desarrollo:
  ```
  docker run -p 8080:8080 -e KC_BOOTSTRAP_ADMIN_USERNAME=admin -e KC_BOOTSTRAP_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:26.0 start-dev
  ```
  (Los nombres de las variables de administrador cambiaron entre versiones de Keycloak: si no entra con `admin/admin`, revisa la documentación de la versión que bajes. Pendiente de confirmar para versiones posteriores a la 26.)
- Alternativa sin Docker: el servidor de demostración de Duende (`https://demo.duendesoftware.com`) tiene un cliente público `interactive.public` con PKCE obligatorio; usuario de prueba y redirect URIs permitidas: Pendiente de confirmar en su página.
- Python 3 y `curl`.

## Pasos
1. En `http://localhost:8080` entra a la consola de administración, crea el realm `lab` y un usuario `ana` con contraseña (desmarca "Temporary").
2. Crea un cliente OpenID Connect `spa-lab`: *Client authentication* **apagado** (cliente público), *Standard flow* encendido, *Valid redirect URIs* `http://localhost:8000/callback` y, en *Advanced*, PKCE method `S256`.
3. Abre `http://localhost:8080/realms/lab/.well-known/openid-configuration` y anota `authorization_endpoint`, `token_endpoint`, `jwks_uri` y `end_session_endpoint`.
4. En Python genera `code_verifier`, `code_challenge` (S256), `state` y `nonce`, e imprime la URL de autorización con `response_type=code`, `client_id`, `redirect_uri`, `scope=openid profile email`, `state`, `nonce`, `code_challenge` y `code_challenge_method=S256`.
5. (Opcional) Corre `python -m http.server 8000` para que el callback no dé error de conexión. Abre la URL, inicia sesión como `ana` y copia de la barra de direcciones `code` y `state`. Verifica que `state` coincide.
6. Canjea el `code` en el `token_endpoint` con `curl` (`grant_type=authorization_code`, `code`, `redirect_uri`, `client_id`, `code_verifier`).
7. Decodifica el `id_token` y el `access_token` (Base64URL) y compara `aud`, `iss`, `exp`, `nonce` y `scope`.
8. Intenta canjear **el mismo** `code` otra vez. Luego repite el flujo y canjea con un `code_verifier` incorrecto.
9. Usa el `refresh_token` para obtener un nuevo access token (`grant_type=refresh_token`).

## Criterios de terminado
- [ ] El canje del paso 6 responde `200` con `access_token`, `id_token`, `refresh_token`, `token_type: Bearer` y `expires_in`.
- [ ] El `nonce` del `id_token` es igual al que generaste y su `aud` es `spa-lab`.
- [ ] Reutilizar el `code` falla con `invalid_grant`.
- [ ] El `code_verifier` incorrecto falla con `invalid_grant` (PKCE verification failed o similar).
- [ ] El refresh del paso 9 devuelve un access token nuevo con `exp` posterior.
- [ ] Explicas por escrito qué evita `state`, qué evita `nonce` y qué evita PKCE.

## Solución

> [!success]- Paso 4: generar PKCE y la URL
> ```python
> import base64, hashlib, secrets, urllib.parse
> verifier = secrets.token_urlsafe(64)
> challenge = base64.urlsafe_b64encode(hashlib.sha256(verifier.encode()).digest()).rstrip(b"=").decode()
> state, nonce = secrets.token_urlsafe(16), secrets.token_urlsafe(16)
> params = {"response_type": "code", "client_id": "spa-lab", "redirect_uri": "http://localhost:8000/callback",
>           "scope": "openid profile email", "state": state, "nonce": nonce,
>           "code_challenge": challenge, "code_challenge_method": "S256"}
> print("verifier:", verifier)
> print("http://localhost:8080/realms/lab/protocol/openid-connect/auth?" + urllib.parse.urlencode(params))
> ```

> [!success]- Paso 6: canje del code
> ```
> curl -s -X POST http://localhost:8080/realms/lab/protocol/openid-connect/token \
>   -d grant_type=authorization_code -d client_id=spa-lab \
>   -d redirect_uri=http://localhost:8000/callback \
>   -d code=PEGA_EL_CODE -d code_verifier=PEGA_EL_VERIFIER
> ```
> El `code` dura muy poco (en Keycloak, alrededor de un minuto por defecto): si tardas, repite el flujo.

> [!success]- Paso 7: diferencias entre tokens
> El `id_token` tiene `aud: spa-lab` (es para el cliente), `nonce` y datos del usuario. El `access_token` es para APIs: su `aud` suele ser `account` u otra audiencia configurada y lleva `scope`. No envíes el `id_token` a un API.

> [!success]- Pasos 8–9 y el porqué
> Un `code` es de un solo uso y está ligado al `code_challenge`: sin el verifier correcto el servidor responde `invalid_grant`. **state** evita CSRF en el callback; **nonce** evita reutilizar un ID token; **PKCE** evita que alguien que intercepte el `code` lo canjee. Para el refresh:
> ```
> curl -s -X POST http://localhost:8080/realms/lab/protocol/openid-connect/token \
>   -d grant_type=refresh_token -d client_id=spa-lab -d refresh_token=PEGA_EL_REFRESH
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
