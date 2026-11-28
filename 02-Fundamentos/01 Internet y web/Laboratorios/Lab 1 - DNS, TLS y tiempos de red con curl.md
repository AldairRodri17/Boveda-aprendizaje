---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Fundamentos
herramienta: curl, nslookup y openssl (incluidos en Windows 10+ y Git Bash)
tags: [fundamentos, laboratorio]
contenido: completo
---
# Lab 1 - DNS, TLS y tiempos de red con curl

Parte de: [[02-Fundamentos/01 Internet y web/_Índice Internet y web|Internet y web]]

**Cubre:** [[Cliente y servidor]] · [[DNS]] · [[HTTP vs HTTPS]] · [[TLS y certificados]] · [[Latencia, ancho de banda y timeouts]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Ver con tus propios ojos cada etapa de una petición HTTPS: resolución DNS, conexión TCP, handshake TLS, primer byte y descarga; e inspeccionar un certificado real.

## Requisitos
- Windows 10/11 con `curl.exe` y `nslookup` (vienen con el sistema) y **Git Bash** (trae `openssl`). En Linux/macOS, los mismos comandos.
- En PowerShell escribe `curl.exe` (no `curl`, que es un alias de `Invoke-WebRequest` en Windows PowerShell 5.1).

## Pasos
1. Resuelve `www.microsoft.com` con `nslookup`. Anota si la respuesta trae alias (CNAME) y cuántas IP devuelve.
2. Repite la consulta preguntando directo a otro resolvedor: `nslookup www.microsoft.com 1.1.1.1`. ¿Cambian las IP?
3. Consulta los registros MX de `gmail.com` (`nslookup -type=mx gmail.com`).
4. Mide los tiempos de una petición:
   ```
   curl.exe -s -o NUL -w "dns=%{time_namelookup} tcp=%{time_connect} tls=%{time_appconnect} ttfb=%{time_starttransfer} total=%{time_total}\n" https://www.wikipedia.org
   ```
   (En Git Bash usa `-o /dev/null`.) Ejecútalo 3 veces y compara la primera con las siguientes.
5. Ejecuta `curl.exe -v https://www.wikipedia.org -o NUL` y localiza en la salida: la IP a la que se conecta, la versión de TLS, el protocolo negociado (ALPN) y el sujeto del certificado.
6. En Git Bash inspecciona el certificado:
   ```
   openssl s_client -connect www.wikipedia.org:443 -servername www.wikipedia.org </dev/null 2>/dev/null | openssl x509 -noout -subject -issuer -dates -ext subjectAltName
   ```
7. Prueba un certificado inválido a propósito con `curl.exe -I https://expired.badssl.com/` y luego `https://wrong.host.badssl.com/`. Lee el error de cada uno.
8. Prueba un timeout: `curl.exe --connect-timeout 2 http://10.255.255.1` y observa cuánto tarda en fallar.

## Criterios de terminado
- [ ] Anotaste las IP y el alias (si hay) de `www.microsoft.com` y los servidores MX de `gmail.com`.
- [ ] Tienes una tabla con `dns`, `tcp`, `tls`, `ttfb` y `total` de 3 ejecuciones y explicas por qué la primera suele ser más lenta.
- [ ] Identificaste en `-v` la versión de TLS y el protocolo ALPN (`h2` o `http/1.1`).
- [ ] Copiaste emisor, vigencia y la lista SAN del certificado de Wikipedia.
- [ ] Explicas con tus palabras la diferencia entre el error de `expired` y el de `wrong.host`.
- [ ] El intento contra `10.255.255.1` falla en ~2 s con un error de timeout de conexión.

## Solución

> [!success]- Pasos 1–3: DNS
> Es común que `www.microsoft.com` devuelva una cadena de alias (CNAME) hacia una CDN antes de la IP final. Otro resolvedor puede devolver IP distintas porque las CDN responden según la ubicación del resolvedor. Los MX de Gmail son varios servidores con distinta prioridad (el número menor se intenta primero).

> [!success]- Paso 4: lectura de los tiempos
> Los valores son acumulados desde el inicio: `tcp − dns` es el tiempo de conexión TCP, `tls − tcp` el handshake TLS, `ttfb − tls` lo que tardó el servidor en empezar a responder y `total − ttfb` la descarga. La primera ejecución suele pagar la resolución DNS sin caché; en las siguientes `dns` baja casi a 0.

> [!success]- Paso 5: qué buscar en `-v`
> Líneas `Connected to ... (IP) port 443`, `SSL connection using TLSv1.3 / ...`, `ALPN: server accepted h2` y `subject: CN=...`. Si ves `h2`, la conexión usa HTTP/2.

> [!success]- Paso 7: errores de certificado
> `expired.badssl.com`: la cadena es válida pero el certificado está fuera de su vigencia. `wrong.host.badssl.com`: el certificado es vigente pero el nombre no aparece en su SAN. En ambos casos curl se niega a continuar; **no** uses `-k` salvo para diagnosticar.

> [!success]- Paso 8: timeout
> `10.255.255.1` es una IP privada que normalmente no responde, así que el SYN se queda sin respuesta y curl corta a los 2 s con `Connection timed out` (código de salida 28). Sin `--connect-timeout` esperaría mucho más.

## Notas personales

_Qué me costó, errores que cometí, dudas._
