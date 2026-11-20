---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Azure
herramienta: Azure CLI, App Service F1 y Key Vault (cuenta gratuita con presupuesto)
tags: [azure, laboratorio]
---
# Lab 4 - Key Vault y Managed Identity

Parte de: [[07-Azure/06 Seguridad/_Índice Seguridad|Seguridad]]

**Cubre:** [[Key Vault]] · [[Managed Identity]] · [[Managed Identity asignada por el sistema]] · [[Azure - RBAC]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Que una web app lea un secreto de Key Vault **sin ningún secreto en su configuración**: identidad administrada + rol RBAC + Key Vault reference.

## Pasos

1. Crea un grupo `rg-lab-kv` y un Key Vault con autorización RBAC.
2. Asígnate el rol *Key Vault Secrets Officer* sobre el vault y crea el secreto `MensajeSecreto`.
3. Crea (o reutiliza del Lab 2) una web app F1 con un endpoint que devuelva la configuración `Mensaje`.
4. Activa la identidad asignada por el sistema y guarda su `principalId`.
5. Asigna a esa identidad el rol *Key Vault Secrets User* con ámbito del vault.
6. Crea el App Setting `Mensaje` con una Key Vault reference al secreto.
7. Verifica en el portal (Configuración de la app) que la referencia aparece resuelta y llama al endpoint.
8. Quita la asignación de rol, reinicia la app y observa qué pasa.
9. Borra el grupo y, si el vault no tiene purge protection, purga el vault eliminado.

## Criterios de terminado

- [ ] El endpoint devuelve el valor del secreto y el App Setting no contiene el valor, solo `@Microsoft.KeyVault(...)`.
- [ ] La referencia aparece con estado correcto (palomita verde) en la configuración de la app.
- [ ] Sin el rol, la referencia muestra error y la app ya no recibe el valor.
- [ ] `az keyvault list-deleted` no muestra el vault al terminar (o sabes por qué sigue ahí).
- [ ] Ningún secreto quedó en tu repositorio ni en el historial de la terminal.

## Solución

> [!success]- Solución pasos 1 y 2
> ```bash
> az group create -n rg-lab-kv -l eastus
> KV=kv-lab-$RANDOM
> az keyvault create -n $KV -g rg-lab-kv --enable-rbac-authorization true
> KV_ID=$(az keyvault show -n $KV --query id -o tsv)
> ME=$(az ad signed-in-user show --query id -o tsv)
> az role assignment create --assignee $ME --role "Key Vault Secrets Officer" --scope $KV_ID
> az keyvault secret set --vault-name $KV -n MensajeSecreto --value "hola desde Key Vault"
> ```
> Si el `secret set` da 403, espera 1–2 minutos: la asignación de rol tarda en propagarse.

> [!success]- Solución pasos 3 a 6
> ```bash
> # endpoint en la API: app.MapGet("/mensaje", (IConfiguration c) => c["Mensaje"]);
> az appservice plan create -n plan-kv -g rg-lab-kv --sku F1 --is-linux
> APP=api-kv-$RANDOM
> az webapp create -n $APP -g rg-lab-kv -p plan-kv --runtime "DOTNETCORE:10.0"
> az webapp deploy -n $APP -g rg-lab-kv --src-path app.zip --type zip
> PID=$(az webapp identity assign -n $APP -g rg-lab-kv --query principalId -o tsv)
> az role assignment create --assignee-object-id $PID --assignee-principal-type ServicePrincipal \
>   --role "Key Vault Secrets User" --scope $KV_ID
> az webapp config appsettings set -n $APP -g rg-lab-kv --settings \
>   "Mensaje=@Microsoft.KeyVault(VaultName=$KV;SecretName=MensajeSecreto)"
> curl https://$APP.azurewebsites.net/mensaje
> ```

> [!success]- Solución pasos 8 y 9
> ```bash
> az role assignment delete --assignee $PID --role "Key Vault Secrets User" --scope $KV_ID
> az webapp restart -n $APP -g rg-lab-kv
> az group delete -n rg-lab-kv --yes
> az keyvault list-deleted -o table
> az keyvault purge -n $KV        # solo si no tiene purge protection
> ```
> La app puede seguir mostrando el valor un rato por caché de las referencias; tras reiniciar y propagarse el cambio, deja de resolverse.

## Notas personales

_Qué me costó, errores que cometí, dudas._
