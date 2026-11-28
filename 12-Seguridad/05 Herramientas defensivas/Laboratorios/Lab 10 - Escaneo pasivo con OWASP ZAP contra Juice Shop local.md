---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: Docker Desktop, ZAP (imagen de Docker), OWASP Juice Shop
tags: [seguridad, laboratorio]
contenido: completo
---
# Lab 10 - Escaneo pasivo con OWASP ZAP contra Juice Shop local

Parte de: [[12-Seguridad/05 Herramientas defensivas/_Índice Herramientas defensivas|Herramientas defensivas]]

**Cubre:** [[OWASP ZAP]] · [[SAST, DAST y SCA]] · [[Configuración insegura]] · [[WAF]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

> [!warning] Ética
> Haz esto solo en tu máquina, en tu propio código o en plataformas creadas para practicar. Probar sistemas ajenos sin autorización por escrito es ilegal. Ver [[Marco legal y autorización por escrito]].

## Objetivo
Correr el escaneo baseline (pasivo) de ZAP contra Juice Shop en una red de Docker aislada, leer el reporte y clasificar los hallazgos en "corregir", "aceptar" o "falso positivo".

## Pasos
1. Crea una red de Docker interna: `docker network create --internal lab-dast`.
2. Levanta Juice Shop en esa red con nombre `juice`.
3. Ejecuta el escaneo baseline de ZAP en la misma red apuntando a `http://juice:3000`, montando una carpeta local para el reporte HTML. Revisa en la documentación de ZAP el nombre actual de la imagen y del script baseline.
4. Abre el reporte y agrupa las alertas por nivel de riesgo.
5. Para tres alertas de configuración (por ejemplo headers faltantes), escribe cómo las corregirías en ASP.NET Core o en el hosting.
6. Explica en tus notas por qué el baseline no envía ataques y qué cambiaría con un escaneo activo.
7. Borra los contenedores y la red al terminar.

## Criterios de terminado
- [ ] La red `lab-dast` es `--internal` (los contenedores no salen a internet).
- [ ] Tienes el reporte HTML de ZAP guardado en la carpeta de este laboratorio.
- [ ] Clasificaste al menos 5 alertas con su justificación.
- [ ] Escribiste la corrección de 3 alertas de configuración.
- [ ] Explicas qué podría y qué no podría bloquear un WAF de esas alertas.

## Solución

> [!success]- Comandos (PowerShell)
> ```powershell
> docker network create --internal lab-dast
> docker run -d --rm --name juice --network lab-dast bkimminich/juice-shop
> docker run --rm --network lab-dast -v "${PWD}:/zap/wrk" -t zaproxy/zap-stable `
>   zap-baseline.py -t http://juice:3000 -r reporte-zap.html
> docker stop juice; docker network rm lab-dast
> ```
> La imagen `zaproxy/zap-stable` y el script `zap-baseline.py` eran los vigentes al escribir esto (verificar en la página oficial).

> [!success]- Corregir headers en ASP.NET Core
> ```csharp
> app.Use(async (ctx, next) =>
> {
>     ctx.Response.Headers["X-Content-Type-Options"] = "nosniff";
>     ctx.Response.Headers["Referrer-Policy"] = "strict-origin-when-cross-origin";
>     ctx.Response.Headers["Content-Security-Policy"] = "default-src 'self'; frame-ancestors 'none'";
>     await next();
> });
> app.UseHsts(); // solo fuera de Development
> ```

> [!success]- WAF y estas alertas
> Los headers faltantes y las cookies sin atributos son configuración de la app o del hosting: un WAF no los corrige (algunos proxies pueden agregar headers, pero la corrección real va en la app). El WAF ayuda contra intentos de inyección, no contra mala configuración.

## Notas personales

_Qué me costó, errores que cometí, dudas._
