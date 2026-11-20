---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: Python 3 (servidor de ejemplo con la librería estándar) y Chrome o Edge con DevTools
tags: [fundamentos, laboratorio]
---
# Lab 1 - Cookies y CORS en el navegador con DevTools

Parte de: [[02-Fundamentos/04 Cookies/_Índice Cookies|Cookies]]

**Cubre:** [[Cookies]] · [[Set-Cookie y el header Cookie]] · [[Cookies - Domain y Path]] · [[Cookies - Expires y Max-Age]] · [[Cookies - Secure y HttpOnly]] · [[Cookies - SameSite]] · [[Cookies de sesión y persistentes]] · [[Cookies de primera y tercera parte]] · [[Cookies entre sistemas propios (subdominios y dominios distintos)]] · [[Mismo origen, mismo sitio y mismo dominio]] · [[Cookies y CORS con credenciales]] · [[Cookies vs localStorage vs sessionStorage]] · [[Cookies vs tokens]] · [[Cookies y CSRF]] · [[Cookies desde JavaScript]] · [[Cookies y SSO]] · [[Restricciones a cookies de terceros en navegadores]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Observar en DevTools cómo se crean, envían y bloquean cookies, y cómo CORS con credenciales decide si JavaScript puede leer una respuesta de otro origen.

## Preparación
Guarda este archivo como `servidor.py` y ábrelo en **dos** terminales: `python servidor.py 8000` (el "front") y `python servidor.py 8001` (el "API"). El API solo permite el origen `http://localhost:8000`.

```python
import http.server, json, sys, uuid
PERMITIDOS = {"http://localhost:8000"}
PAGINA = """<!doctype html><meta charset="utf-8"><title>Lab cookies</title>
<h1>Front en <span id="o"></span></h1>
<button onclick="llamar('omit')">fetch sin credenciales</button>
<button onclick="llamar('include')">fetch con credentials: 'include'</button>
<button onclick="post()">POST JSON (preflight)</button>
<pre id="r"></pre>
<script>
document.getElementById('o').textContent = location.origin;
const API = 'http://localhost:8001/api/yo';
async function llamar(c) {
  try { const r = await fetch(API, {credentials: c}); document.getElementById('r').textContent = r.status + ' ' + await r.text(); }
  catch (e) { document.getElementById('r').textContent = 'Error: ' + e; }
}
async function post() {
  try { const r = await fetch(API, {method: 'POST', credentials: 'include', headers: {'Content-Type': 'application/json'}, body: '{}'});
        document.getElementById('r').textContent = r.status + ' ' + await r.text(); }
  catch (e) { document.getElementById('r').textContent = 'Error: ' + e; }
}
</script>"""
class H(http.server.BaseHTTPRequestHandler):
    def cors(self):
        o = self.headers.get("Origin")
        if o in PERMITIDOS:
            self.send_header("Access-Control-Allow-Origin", o)
            self.send_header("Access-Control-Allow-Credentials", "true")
            self.send_header("Vary", "Origin")
    def do_OPTIONS(self):
        self.send_response(204); self.cors()
        self.send_header("Access-Control-Allow-Methods", "GET, POST")
        self.send_header("Access-Control-Allow-Headers", "Content-Type")
        self.end_headers()
    def do_GET(self):
        if self.path == "/":
            b = PAGINA.encode(); self.send_response(200)
            self.send_header("Content-Type", "text/html; charset=utf-8")
        elif self.path == "/login":
            b = b"Sesion iniciada"; self.send_response(200)
            self.send_header("Set-Cookie", f"sid={uuid.uuid4().hex}; Path=/; HttpOnly; SameSite=Lax")
            self.send_header("Set-Cookie", "tema=oscuro; Path=/; Max-Age=3600; SameSite=Lax")
            self.send_header("Content-Type", "text/plain; charset=utf-8")
        elif self.path == "/logout":
            b = b"Sesion cerrada"; self.send_response(200)
            self.send_header("Set-Cookie", "sid=; Path=/; Max-Age=0")
            self.send_header("Content-Type", "text/plain; charset=utf-8")
        elif self.path == "/api/yo":
            b = json.dumps({"cookie_recibida": self.headers.get("Cookie")}).encode(); self.send_response(200)
            self.send_header("Content-Type", "application/json"); self.cors()
        else:
            b = b"No existe"; self.send_response(404)
        self.send_header("Content-Length", str(len(b))); self.end_headers(); self.wfile.write(b)
    do_POST = do_GET
if __name__ == "__main__":
    puerto = int(sys.argv[1])
    print(f"Escuchando en http://localhost:{puerto}")
    http.server.ThreadingHTTPServer(("0.0.0.0", puerto), H).serve_forever()
```

## Pasos
1. Abre `http://localhost:8001/login`. En DevTools → **Application → Cookies** revisa `sid` y `tema`: columnas `HttpOnly`, `SameSite`, `Expires / Max-Age`. ¿Cuál es de sesión y cuál persistente?
2. En la consola de esa pestaña ejecuta `document.cookie`. ¿Cuál cookie ves y cuál no?
3. Abre `http://localhost:8000/` (otro puerto). Revisa *Application → Cookies*: ¿aparecen también ahí? Pulsa **fetch sin credenciales** y luego **fetch con credentials: 'include'**. En **Network**, compara el header `Cookie` de ambas peticiones y lo que muestra la página.
4. Pulsa **POST JSON (preflight)**. En Network localiza la petición `OPTIONS` previa y sus headers `Access-Control-Request-*` / `Access-Control-Allow-*`.
5. Ahora abre la misma página como `http://127.0.0.1:8000/` y repite el paso 3. Lee el error de la consola y revisa en Network si se envió la cookie.
6. Edita `servidor.py` y agrega `"http://127.0.0.1:8000"` a `PERMITIDOS`. Reinicia el 8001 y repite desde `127.0.0.1:8000`. ¿Ya llega la respuesta? ¿Y la cookie?
7. Cambia temporalmente `cors()` para responder `Access-Control-Allow-Origin: *` sin quitar `Allow-Credentials`. Repite el fetch con credenciales y lee el error.
8. Desde la consola de `localhost:8000` crea una cookie con `document.cookie = "idioma=es-MX; Max-Age=60; Path=/; SameSite=Lax"` y guarda algo en `localStorage` y `sessionStorage`. Abre `localhost:8001` en otra pestaña y compara qué ves en cada almacén.
9. Visita `http://localhost:8001/logout` y confirma que `sid` desaparece.

## Criterios de terminado
- [ ] Explicas por qué `document.cookie` muestra `tema` pero no `sid`.
- [ ] Mostraste que las cookies de `localhost:8001` también aparecen en `localhost:8000` y explicas por qué (las cookies no distinguen puerto).
- [ ] En el paso 3, la petición sin credenciales no lleva `Cookie` y la petición con `include` sí, y la página muestra `cookie_recibida` con `sid`.
- [ ] Identificaste el preflight `OPTIONS` del paso 4 y respondió `204` con `Access-Control-Allow-Methods`.
- [ ] Desde `127.0.0.1:8000` documentaste dos efectos distintos: el error de CORS y la cookie `SameSite=Lax` que no se envía porque es otro sitio.
- [ ] Copiaste el mensaje de error del paso 7 y explicas la regla.
- [ ] Tras `/logout`, `sid` ya no aparece en DevTools.

## Solución

> [!success]- Pasos 1–2
> `sid` no tiene `Max-Age` (cookie de sesión) y es `HttpOnly`, por eso JavaScript no la ve. `tema` es persistente (1 h) y legible desde JS.

> [!success]- Paso 3: mismo sitio, distinto origen
> `localhost:8000` y `localhost:8001` son **orígenes distintos** (puerto) pero el **mismo sitio**, y las cookies se asocian al host sin puerto: por eso las cookies del 8001 se ven también en el 8000. Sin `credentials: 'include'`, `fetch` a otro origen no envía cookies; con `include` las envía y, como el API responde con el origen exacto y `Allow-Credentials: true`, el navegador deja leer la respuesta.

> [!success]- Paso 4: preflight
> `Content-Type: application/json` no es un tipo "simple", así que el navegador primero envía `OPTIONS` con `Origin`, `Access-Control-Request-Method: POST` y `Access-Control-Request-Headers: content-type`. Solo si la respuesta lo permite manda el `POST`.

> [!success]- Pasos 5–6: otro sitio
> `127.0.0.1` y `localhost` son **sitios distintos**. Primero falla CORS: la respuesta no trae `Access-Control-Allow-Origin` para `http://127.0.0.1:8000` y la consola muestra un error de política CORS (la petición sí llegó al servidor). Al agregar el origen, la respuesta ya se puede leer, pero `cookie_recibida` sale `null`: `SameSite=Lax` no se envía en un `fetch` entre sitios. Para eso haría falta `SameSite=None; Secure` (HTTPS) y que el navegador no bloquee cookies de terceros: justo lo que hoy se restringe.

> [!success]- Paso 7: comodín con credenciales
> El navegador rechaza la respuesta con un mensaje del tipo *"The value of the 'Access-Control-Allow-Origin' header in the response must not be the wildcard '*' when the request's credentials mode is 'include'"*. Con credenciales siempre debe ir el origen exacto.

> [!success]- Paso 8: almacenes
> La cookie `idioma` aparece en ambos puertos (las cookies no separan por puerto). `localStorage` y `sessionStorage` sí están separados por **origen**, así que en `localhost:8001` no ves lo guardado desde `localhost:8000`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
