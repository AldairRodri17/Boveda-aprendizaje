---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Power BI
herramienta: Power BI Desktop o Excel (Power Query) + JSONPlaceholder
tags: [power-bi, laboratorio]
---
# Lab 3 - Consumir un API en Power Query

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

**Cubre:** [[Power Query - consumir un API]] · [[Power Query - parámetros]] · [[Power Query - funciones personalizadas]] · [[Power Query - manejo de errores]] · [[Lenguaje M - let e in]] · [[Power Query en Excel (consumir un API desde Excel)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Consumir el API público de prueba `https://jsonplaceholder.typicode.com` (sin autenticación): usuarios y sus publicaciones, con una función parametrizada y manejo de errores, escrito para que pueda actualizarse en el Service.

## Pasos

1. Crea un parámetro `UrlBase` (texto) = `https://jsonplaceholder.typicode.com`.
2. Con *Consulta en blanco* y el Editor avanzado, trae `users` con `Web.Contents(UrlBase, [RelativePath = "users"])` y `Json.Document`.
3. Convierte a tabla y quédate con `id`, `name`, `email`; expande `address` para traer `city`.
4. Escribe la función `fnPosts(userId as number)` que use `Query = [userId = ...]` y devuelva una tabla.
5. En la tabla de usuarios, *Invocar función personalizada* con `id` y expande `title`.
6. Prueba el manejo de errores: llama a `fnPosts` también con un recurso inexistente (`RelativePath = "postsX"`) envuelto en `try ... otherwise` para que devuelva una tabla vacía.
7. Asigna tipos y carga. Repite los pasos 1–3 en Excel (*Datos → Obtener datos → De otras fuentes → Consulta en blanco*) pegando el código.

## Criterios de terminado

- [ ] La tabla de usuarios tiene 10 filas con `id`, `name`, `email`, `city`.
- [ ] La tabla de publicaciones expandida tiene 100 filas (10 por usuario).
- [ ] Ninguna consulta concatena la URL completa: todas usan `UrlBase` + `RelativePath`/`Query`.
- [ ] Con el recurso inexistente la consulta no falla: devuelve 0 filas para ese caso.
- [ ] La misma consulta de usuarios funciona en Excel.

## Solución

> [!success]- Usuarios
> ```m
> let
>     J = Json.Document(Web.Contents(UrlBase, [RelativePath = "users"])),
>     T = Table.FromRecords(J),
>     Sel = Table.SelectColumns(T, {"id", "name", "email", "address"}),
>     Dir = Table.ExpandRecordColumn(Sel, "address", {"city"}),
>     Tipos = Table.TransformColumnTypes(Dir, {{"id", Int64.Type}, {"name", type text}, {"email", type text}, {"city", type text}})
> in
>     Tipos
> ```

> [!success]- fnPosts con manejo de errores
> ```m
> (userId as number, optional recurso as text) as table =>
> let
>     R = try Json.Document(Web.Contents(UrlBase,
>             [RelativePath = if recurso = null then "posts" else recurso,
>              Query = [userId = Text.From(userId)]])) otherwise {},
>     T = if List.IsEmpty(R) then #table({"userId", "id", "title", "body"}, {}) else Table.FromRecords(R)
> in
>     T
> ```

> [!success]- Por qué RelativePath
> El Service revisa la URL base para validar credenciales; si la URL se arma con `&`, se considera origen dinámico y la actualización programada falla.

## Notas personales

_Qué me costó, errores que cometí, dudas._
