---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Azure
herramienta: Bicep con Azure CLI y VS Code (extensión de Bicep)
tags: [azure, laboratorio]
---
# Lab 6 - Bicep básico

Parte de: [[07-Azure/10 Infraestructura como código/_Índice Infraestructura como código|Infraestructura como código]]

**Cubre:** [[Bicep]] · [[ARM templates]] · [[Azure Portal vs CLI vs IaC]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Escribir una plantilla Bicep con parámetros, variables, un recurso y salidas; ver el ARM generado, usar `what-if`, desplegar, cambiar y limpiar.

> [!warning] Dónde hacerlo
> En el **sandbox de Microsoft Learn** (sin tarjeta) o en tu cuenta gratuita con un presupuesto y alertas configurados. El sandbox trae un grupo de recursos ya creado y permisos limitados (verificar en la página oficial).

## Pasos

1. Crea `main.bicep` con parámetros `prefijo` (3–11 caracteres), `location` (por defecto la del grupo) y `sku` (permitidos `Standard_LRS` y `Standard_GRS`).
2. Declara una variable con un nombre único en minúsculas a partir del prefijo y `uniqueString(resourceGroup().id)`.
3. Declara una cuenta de almacenamiento `StorageV2` con TLS 1.2 mínimo, sin acceso público a blobs y etiquetas `ambiente=lab`.
4. Agrega salidas con el nombre de la cuenta y su endpoint de blobs.
5. Compila con `az bicep build` y revisa el JSON de ARM generado.
6. Ejecuta `what-if` contra el grupo del sandbox y luego despliega.
7. Cambia la etiqueta o el SKU, repite `what-if` y observa que marca *Modify* (no *Create*).
8. Despliega de nuevo dos veces seguidas y comprueba que el segundo despliegue no cambia nada (idempotencia).
9. Borra la cuenta creada.

## Criterios de terminado

- [ ] `az bicep build -f main.bicep` genera `main.json` sin errores.
- [ ] `what-if` muestra 1 recurso a crear la primera vez y *Modify* tras el cambio.
- [ ] `az deployment group show ... --query properties.outputs` muestra el nombre y el endpoint.
- [ ] Un despliegue repetido sin cambios no altera el recurso (what-if: *NoChange*).
- [ ] El grupo queda vacío al final.

## Solución

> [!success]- Solución: main.bicep
> ```bicep
> @minLength(3)
> @maxLength(11)
> param prefijo string
> param location string = resourceGroup().location
> @allowed([
>   'Standard_LRS'
>   'Standard_GRS'
> ])
> param sku string = 'Standard_LRS'
>
> var nombre = toLower('${prefijo}${uniqueString(resourceGroup().id)}')
>
> resource st 'Microsoft.Storage/storageAccounts@2023-05-01' = {
>   name: nombre
>   location: location
>   sku: {
>     name: sku
>   }
>   kind: 'StorageV2'
>   tags: {
>     ambiente: 'lab'
>     herramienta: 'bicep'
>   }
>   properties: {
>     minimumTlsVersion: 'TLS1_2'
>     allowBlobPublicAccess: false
>     supportsHttpsTrafficOnly: true
>   }
> }
>
> output nombreCuenta string = st.name
> output endpointBlob string = st.properties.primaryEndpoints.blob
> ```
> `uniqueString` da 13 caracteres: con un prefijo de hasta 11 el nombre no pasa de 24.

> [!success]- Solución: comandos
> ```bash
> RG=$(az group list --query "[0].name" -o tsv)
> az bicep build -f main.bicep
> az deployment group what-if -g $RG -f main.bicep -p prefijo=lab
> az deployment group create -g $RG -n bicep-lab -f main.bicep -p prefijo=lab
> az deployment group show -g $RG -n bicep-lab --query properties.outputs
> az deployment group what-if -g $RG -f main.bicep -p prefijo=lab sku=Standard_GRS
> NOMBRE=$(az deployment group show -g $RG -n bicep-lab --query properties.outputs.nombreCuenta.value -o tsv)
> az storage account delete -n $NOMBRE -g $RG --yes
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
