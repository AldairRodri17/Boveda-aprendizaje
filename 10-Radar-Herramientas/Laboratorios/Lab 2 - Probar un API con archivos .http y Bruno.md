---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Radar de herramientas
herramienta: VS Code (REST Client) o Visual Studio, Bruno
tags: [radar-herramientas, laboratorio]
contenido: completo
---
# Lab 2 - Probar un API con archivos .http y Bruno

Parte de: [[10-Radar-Herramientas/_Índice Radar de herramientas|Radar de herramientas]]

**Cubre:** [[Postman, Bruno y archivos .http]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Guardar en el repositorio las peticiones de un API (crear, leer, error) como archivos `.http` y como colección de Bruno, sin secretos versionados.

## Pasos
1. Crea un API mínimo (`dotnet new webapi -n Demo`) o usa uno tuyo con al menos un `GET` y un `POST`.
2. Crea `Demo.http` con variables (`@base`) y tres peticiones: `GET` lista, `POST` creación con JSON, `POST` inválido que debe devolver 400.
3. Ejecútalas desde VS Code (extensión REST Client) o Visual Studio y revisa estado, headers y cuerpo.
4. Encadena: guarda el `id` de la respuesta del `POST` y úsalo en un `GET /recurso/{id}`.
5. Instala Bruno, crea una colección dentro de la carpeta del repo y reproduce las mismas peticiones; agrega una aserción de estado en cada una.
6. Mueve el token o la clave a un archivo de entorno ignorado por git y verifica con `git status` que no se versiona.
7. Anota en Notas personales cuál prefieres y por qué.

## Criterios de terminado
- [ ] `Demo.http` ejecuta las 3 peticiones; la inválida devuelve 400.
- [ ] El `GET` por id usa el valor devuelto por el `POST` sin copiarlo a mano.
- [ ] La colección de Bruno vive en el repo como archivos de texto y sus aserciones pasan.
- [ ] Ningún secreto aparece en `git diff` ni en los archivos versionados.

## Solución

> [!success]- Archivo .http (sintaxis de REST Client)
> ```http
> @base = https://localhost:5001
>
> ### Listar
> GET {{base}}/productos
>
> ### Crear
> # @name crear
> POST {{base}}/productos
> Content-Type: application/json
>
> { "nombre": "Teclado", "precio": 500 }
>
> ### Obtener el creado
> GET {{base}}/productos/{{crear.response.body.$.id}}
>
> ### Inválido (espera 400)
> POST {{base}}/productos
> Content-Type: application/json
>
> { "nombre": "", "precio": -1 }
> ```
> El encadenamiento con `# @name` funciona en REST Client; en Visual Studio la sintaxis de variables de respuesta puede diferir (Pendiente de confirmar en tu versión).

> [!success]- Secretos fuera del repo
> En REST Client usa `$dotenv` o `rest-client.environmentVariables` en la configuración de usuario; en Bruno, variables secretas del entorno o un `.env` en `.gitignore`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
