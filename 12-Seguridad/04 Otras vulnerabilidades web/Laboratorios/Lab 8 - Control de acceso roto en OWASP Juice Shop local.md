---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Seguridad
herramienta: Docker Desktop, OWASP Juice Shop, DevTools o Burp Suite Community
tags: [seguridad, laboratorio]
---
# Lab 8 - Control de acceso roto en OWASP Juice Shop local

Parte de: [[12-Seguridad/04 Otras vulnerabilidades web/_Índice Otras vulnerabilidades web|Otras vulnerabilidades web]]

**Cubre:** [[Control de acceso roto e IDOR]] · [[Seguridad de APIs]] · [[Exposición de datos sensibles]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

> [!warning] Ética
> Haz esto solo en tu máquina, en tu propio código o en plataformas creadas para practicar. Probar sistemas ajenos sin autorización por escrito es ilegal. Ver [[Marco legal y autorización por escrito]].

## Objetivo
Levantar OWASP Juice Shop en tu máquina, encontrar el marcador de puntuación y resolver retos de la categoría **Broken Access Control** de dificultad baja (por ejemplo, ver la canasta de otro usuario), y escribir cómo se corregiría cada uno en una API .NET.

## Pasos
1. Levanta Juice Shop: `docker run --rm -p 127.0.0.1:3000:3000 bkimminich/juice-shop` (escuchando solo en tu equipo).
2. Abre `http://localhost:3000`, crea dos usuarios propios y busca el Score Board (es el primer reto).
3. Filtra el Score Board por la categoría Broken Access Control y elige retos de 1–2 estrellas.
4. Con las DevTools (pestaña Red) o Burp, observa las peticiones a la API REST que hace la tienda cuando ves tu propia canasta.
5. Resuelve los retos usando solo tus dos usuarios. Usa las pistas del Score Board si te atoras.
6. Para cada reto resuelto, escribe la verificación que faltaba en el servidor y cómo se vería en un endpoint de ASP.NET Core.
7. Al terminar, detén el contenedor (`Ctrl+C`; con `--rm` se borra solo).

## Criterios de terminado
- [ ] Juice Shop corre solo en `127.0.0.1:3000` (no accesible desde otra máquina de la red).
- [ ] El Score Board muestra al menos 2 retos de Broken Access Control resueltos.
- [ ] Para cada uno tienes la causa (qué verificación faltaba) y la corrección en C#.
- [ ] Puedes explicar por qué ocultar un botón en el front no es control de acceso.

## Solución

> No se incluyen los pasos de cada reto: Juice Shop trae pistas y existe un libro oficial gratuito del proyecto. La lectura defensiva es esta.

> [!success]- Causa común
> La API devolvía o modificaba el recurso indicado por el id de la petición sin comprobar que pertenecía al usuario autenticado. El front solo mostraba "tus" datos, pero el servidor no lo exigía.

> [!success]- Corrección en ASP.NET Core
> ```csharp
> app.MapGet("/api/canastas/{id:int}", async (int id, ClaimsPrincipal user, ICanastas repo) =>
> {
>     var userId = user.FindFirstValue(ClaimTypes.NameIdentifier);
>     var canasta = await repo.ObtenerAsync(id);
>     return canasta is null || canasta.UsuarioId != userId
>         ? Results.NotFound()            // 404 para no confirmar que existe
>         : Results.Ok(CanastaDto.De(canasta));
> }).RequireAuthorization();
> ```
> Mejor aún: que el repositorio filtre por usuario en la propia consulta (`WHERE Id = @Id AND UsuarioId = @UsuarioId`).

## Notas personales

_Qué me costó, errores que cometí, dudas._
