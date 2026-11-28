---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Azure
herramienta: .NET SDK, Azure CLI y App Service (plan F1)
tags: [azure, laboratorio]
contenido: completo
---
# Lab 2 - Desplegar una API .NET a App Service

Parte de: [[07-Azure/03 Cómputo/_Índice Cómputo|Cómputo]]

**Cubre:** [[App Service]] · [[App Service - planes y escalado]] · [[Azure CLI]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Publicar una API mínima de .NET en App Service con zip deploy, configurarla con App Settings y verificarla con un health check, como alternativa repetible a publicar en IIS por FTP.

> [!warning] Dónde hacerlo
> En el **sandbox de Microsoft Learn** (sin tarjeta) o en tu cuenta gratuita con un presupuesto y alertas configurados. El sandbox trae un grupo de recursos ya creado y permisos limitados (verificar en la página oficial).

## Pasos

1. Crea una API con `dotnet new webapi -n LabApi` y agrega un endpoint `GET /health` que responda `ok` y otro `GET /saludo` que lea la configuración `Saludo:Texto`.
2. Pruébala en local con `dotnet run` y `curl`.
3. Publica en Release a una carpeta y comprímela en `app.zip` (el contenido de la carpeta, no la carpeta).
4. Consulta los runtimes disponibles con `az webapp list-runtimes --os linux` y elige el de tu versión de .NET.
5. Crea un plan Linux F1 y una web app con ese runtime (en el grupo del sandbox o en `rg-lab-app`).
6. Despliega `app.zip` con `az webapp deploy`.
7. Configura el App Setting `Saludo__Texto` sin volver a desplegar.
8. Revisa el Log stream si algo falla; al terminar, borra la app y el plan (o el grupo).

## Criterios de terminado

- [ ] `curl https://<app>.azurewebsites.net/health` devuelve 200 y `ok`.
- [ ] `/saludo` devuelve el texto del App Setting, no el de `appsettings.json`.
- [ ] Puedes explicar por qué en Linux se usa `__` en vez de `:`.
- [ ] Al final, `az webapp list -o table` ya no muestra la app.

## Solución

> [!success]- Solución: código de la API (Program.cs, agrega antes de app.Run())
> ```csharp
> app.MapGet("/health", () => Results.Ok("ok"));
> app.MapGet("/saludo", (IConfiguration cfg) => cfg["Saludo:Texto"] ?? "sin configurar");
> ```

> [!success]- Solución: publicar y crear recursos
> ```bash
> dotnet publish -c Release -o publish
> cd publish && zip -r ../app.zip . && cd ..
> RG=$(az group list --query "[0].name" -o tsv)   # o rg-lab-app en tu suscripción
> APP=api-lab-$RANDOM
> az appservice plan create -n plan-lab -g $RG --sku F1 --is-linux
> az webapp list-runtimes --os linux -o tsv | grep -i dotnet
> az webapp create -n $APP -g $RG -p plan-lab --runtime "DOTNETCORE:10.0"   # usa el que listaste
> az webapp deploy -n $APP -g $RG --src-path app.zip --type zip
> curl https://$APP.azurewebsites.net/health
> ```

> [!success]- Solución: App Setting y limpieza
> ```bash
> az webapp config appsettings set -n $APP -g $RG --settings Saludo__Texto="Hola desde App Service"
> curl https://$APP.azurewebsites.net/saludo
> az webapp log tail -n $APP -g $RG    # si algo falla
> az webapp delete -n $APP -g $RG
> az appservice plan delete -n plan-lab -g $RG --yes
> ```
> Si F1 tarda en responder la primera vez es normal: sin *Always On* la app se duerme.

## Notas personales

_Qué me costó, errores que cometí, dudas._
