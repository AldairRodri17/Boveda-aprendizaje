---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: Python 3 con PyJWT y cryptography (pip install pyjwt cryptography) o Google Colab
tags: [fundamentos, laboratorio]
---
# Lab 1 - Decodificar y validar un JWT

Parte de: [[02-Fundamentos/05 Autenticación y autorización/_Índice Autenticación y autorización|Autenticación y autorización]]

**Cubre:** [[Tokens de acceso]] · [[JWT y autenticación con Bearer]] · [[Firmas de JWT (HS256 vs RS256) y JWKS]] · [[Rotación de llaves de firma]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Entender qué hay dentro de un JWT y comprobar, con código, qué validaciones lo hacen confiable: firma, algoritmo, emisor, audiencia y expiración.

## Requisitos
- Python 3 local (`pip install pyjwt cryptography`) o un cuaderno de **Google Colab**.
- No pegues tokens reales de trabajo en sitios web para decodificarlos.

## Pasos
1. Genera un par de llaves RSA de 2048 bits con `cryptography`.
2. Emite un JWT `RS256` con header `kid: "llave-1"` y claims `sub`, `name`, `iss = "https://emisor.local"`, `aud = "api-pedidos"`, `iat`, `exp` (5 min) y `scope`.
3. Sin usar PyJWT, separa el token por puntos y decodifica header y payload con Base64URL (ojo con el relleno `=`).
4. Escribe `validar(token)` con `jwt.decode` que exija `RS256`, la audiencia, el emisor y una tolerancia de reloj de 30 s.
5. Prueba que `validar` **rechace**: (a) el token con el payload alterado (cambia `scope` y vuelve a codificar sin firmar), (b) audiencia distinta, (c) token expirado, (d) token con `alg: none`, (e) token firmado `HS256` con un secreto.
6. Publica tu llave como JWK: exporta la llave pública con `jwt.algorithms.RSAAlgorithm.to_jwk(publica)` y arma un JWKS `{"keys": [...]}` con su `kid`.
7. Simula una rotación: crea `llave-2`, firma un token nuevo con ella y haz que tu validador elija la llave del JWKS según el `kid` del header (`jwt.get_unverified_header`).

## Criterios de terminado
- [ ] Imprimes header y payload decodificados a mano y coinciden con lo que emitiste.
- [ ] El token legítimo pasa la validación.
- [ ] Los cinco casos del paso 5 lanzan, respectivamente, `InvalidSignatureError`, `InvalidAudienceError`, `ExpiredSignatureError` e `InvalidAlgorithmError` (los dos últimos).
- [ ] Tu JWKS contiene `kty`, `n`, `e` y `kid` y ninguna parte privada (`d`, `p`, `q`).
- [ ] Tras la rotación se validan tokens de `llave-1` y de `llave-2` mientras ambas estén en el JWKS, y falla uno con un `kid` desconocido.

## Solución

> [!success]- Script de referencia (pasos 1–5)
> ```python
> import base64, json, time, jwt
> from cryptography.hazmat.primitives.asymmetric import rsa
> from cryptography.hazmat.primitives import serialization
>
> def b64url_decode(s):
>     return base64.urlsafe_b64decode(s + "=" * (-len(s) % 4))
>
> privada = rsa.generate_private_key(public_exponent=65537, key_size=2048)
> publica = privada.public_key()
> ahora = int(time.time())
> claims = {"sub": "123", "name": "Ana", "iss": "https://emisor.local", "aud": "api-pedidos",
>           "iat": ahora, "exp": ahora + 300, "scope": "pedidos.leer"}
> token = jwt.encode(claims, privada, algorithm="RS256", headers={"kid": "llave-1"})
> print(token[:40], "...")
> h, p, f = token.split(".")
> print(json.loads(b64url_decode(h)))
> print(json.loads(b64url_decode(p)))
>
> def validar(t, aud="api-pedidos"):
>     return jwt.decode(t, publica, algorithms=["RS256"], audience=aud, issuer="https://emisor.local", leeway=30)
> print("valido:", validar(token)["sub"])
> # alterar payload
> p2 = json.loads(b64url_decode(p)); p2["scope"] = "pedidos.admin"
> p2b = base64.urlsafe_b64encode(json.dumps(p2).encode()).rstrip(b"=").decode()
> for nombre, t, kw in [("alterado", f"{h}.{p2b}.{f}", {}),
>                       ("aud", token, {"aud": "otra-api"}),
>                       ("expirado", jwt.encode({**claims, "exp": ahora - 3600}, privada, algorithm="RS256"), {}),
>                       ("none", jwt.encode(claims, None, algorithm="none"), {}),
>                       ("hs256", jwt.encode(claims, "secreto-de-pruebas-de-32-bytes!!", algorithm="HS256"), {})]:
>     try:
>         validar(t, **kw); print(nombre, "ACEPTADO (mal)")
>     except Exception as e:
>         print(nombre, "->", type(e).__name__)
> ```
> Salida esperada en los rechazos: `alterado -> InvalidSignatureError`, `aud -> InvalidAudienceError`, `expirado -> ExpiredSignatureError`, `none -> InvalidAlgorithmError`, `hs256 -> InvalidAlgorithmError`. El último muestra por qué hay que fijar `algorithms=["RS256"]`: el validador no debe confiar en el `alg` del header.

> [!success]- Pasos 6–7: JWKS y rotación
> ```python
> import json, jwt
> def jwk(pub, kid):
>     d = json.loads(jwt.algorithms.RSAAlgorithm.to_jwk(pub)); d.update(kid=kid, use="sig", alg="RS256"); return d
> jwks = {"keys": [jwk(publica, "llave-1"), jwk(publica2, "llave-2")]}
>
> def validar_con_jwks(t):
>     kid = jwt.get_unverified_header(t)["kid"]
>     k = next((k for k in jwks["keys"] if k["kid"] == kid), None)
>     if k is None:
>         raise ValueError("kid desconocido: refrescar JWKS o rechazar")
>     llave = jwt.algorithms.RSAAlgorithm.from_jwk(json.dumps(k))
>     return jwt.decode(t, llave, algorithms=["RS256"], audience="api-pedidos", issuer="https://emisor.local")
> ```
> El `kid` del header **solo** sirve para elegir la llave; la seguridad viene de validar la firma con una llave de tu JWKS de confianza. Así se rota sin cortar: publicar `llave-2`, empezar a firmar con ella y retirar `llave-1` cuando expiren sus tokens.

## Notas personales

_Qué me costó, errores que cometí, dudas._
