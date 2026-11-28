---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: .NET SDK, Microsoft.AspNetCore.Authentication.JwtBearer, archivo .http o curl
tags: [seguridad, laboratorio]
contenido: completo
---
# Lab 7 - Revisión de un JWT mal validado en tu propio código

Parte de: [[12-Seguridad/04 Otras vulnerabilidades web/_Índice Otras vulnerabilidades web|Otras vulnerabilidades web]]

**Cubre:** [[Ataques a JWT y cómo evitarlos]] · [[Autenticación rota]] · [[Seguridad de APIs]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

> [!warning] Ética
> Haz esto solo en tu máquina, en tu propio código o en plataformas creadas para practicar. Probar sistemas ajenos sin autorización por escrito es ilegal. Ver [[Marco legal y autorización por escrito]].

## Objetivo
Escribir una API tuya con una validación de JWT deliberadamente débil, comprobar con tokens que tú mismo generas que acepta tokens que debería rechazar, y dejar la configuración correcta con pruebas que lo demuestren.

## Pasos
1. Crea una minimal API con `AddJwtBearer` y un endpoint protegido `GET /yo` que devuelva los claims.
2. Agrega un endpoint `POST /token` (solo en Development) que emita tokens firmados con HS256 y una llave de 32 bytes guardada en User Secrets, con `iss`, `aud` y `exp` de 5 minutos.
3. Configura la validación **débil** a propósito: `ValidateLifetime = false`, `ValidateAudience = false`, `ValidateIssuer = false`.
4. Con un script tuyo (C# o Python con PyJWT) genera tres tokens de prueba: uno ya expirado, uno con otra `aud` y uno firmado con **otra** llave. Envía cada uno a `/yo` y anota cuáles acepta.
5. Corrige `TokenValidationParameters`: valida emisor, audiencia, vigencia (con `ClockSkew` de 1 minuto), firma y fija `ValidAlgorithms`.
6. Repite el paso 4 y escribe pruebas de integración (`WebApplicationFactory`) que esperen 401 en los tres casos y 200 con un token correcto.

## Criterios de terminado
- [ ] Con la configuración débil, el token expirado y el de otra audiencia devuelven 200 (documentado).
- [ ] Con la configuración corregida, los tres tokens malos devuelven 401.
- [ ] Un token válido recién emitido devuelve 200.
- [ ] La llave de firma no está en el código ni en `appsettings.json`.
- [ ] Las pruebas automáticas pasan con `dotnet test`.

## Solución

> [!success]- Configuración correcta
> ```csharp
> var llave = new SymmetricSecurityKey(Convert.FromBase64String(builder.Configuration["Jwt:Key"]!));
> builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
>     .AddJwtBearer(o => o.TokenValidationParameters = new TokenValidationParameters
>     {
>         ValidateIssuer = true, ValidIssuer = "lab-jwt",
>         ValidateAudience = true, ValidAudience = "lab-api",
>         ValidateLifetime = true, ClockSkew = TimeSpan.FromMinutes(1),
>         ValidateIssuerSigningKey = true, IssuerSigningKey = llave,
>         ValidAlgorithms = new[] { SecurityAlgorithms.HmacSha256 }
>     });
> ```

> [!success]- Generar tokens de prueba con Python (PyJWT)
> ```python
> import jwt, time, base64, os
> buena = base64.b64decode(os.environ["LAB_JWT_KEY"])
> otra = os.urandom(32)
> base = {"sub": "ana", "iss": "lab-jwt", "aud": "lab-api"}
> expirado = jwt.encode({**base, "exp": int(time.time()) - 600}, buena, "HS256")
> otra_aud = jwt.encode({**base, "aud": "otra-api", "exp": int(time.time()) + 300}, buena, "HS256")
> otra_llave = jwt.encode({**base, "exp": int(time.time()) + 300}, otra, "HS256")
> print(expirado, otra_aud, otra_llave, sep="\n")
> ```
> Son tokens para **tu** API local; sirven para verificar que la validación los rechaza.

> [!success]- Qué esperar con la configuración débil
> - Expirado: aceptado (no se valida `exp`).
> - Otra audiencia: aceptado (no se valida `aud`).
> - Otra llave: rechazado, porque la firma sí se sigue validando. Si alguna vez desactivas también la validación de la firma, cualquiera podría fabricar tokens: por eso `ValidateIssuerSigningKey` y la llave nunca se tocan.

## Notas personales

_Qué me costó, errores que cometí, dudas._
