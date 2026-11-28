---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Azure
herramienta: Azure CLI y curl (sandbox de Microsoft Learn)
tags: [azure, laboratorio]
contenido: completo
---
# Lab 3 - Blob Storage con SAS

Parte de: [[07-Azure/04 Datos/_Índice Datos|Datos]]

**Cubre:** [[Azure Blob Storage]] · [[Blob Storage - SAS (firmas de acceso compartido)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Subir un archivo a un contenedor privado y compartirlo con una SAS de solo lectura y corta duración; comprobar qué pasa sin SAS, al intentar escribir y al expirar.

> [!warning] Dónde hacerlo
> En el **sandbox de Microsoft Learn** (sin tarjeta) o en tu cuenta gratuita con un presupuesto y alertas configurados. El sandbox trae un grupo de recursos ya creado y permisos limitados (verificar en la página oficial).

## Pasos

1. Crea una cuenta de almacenamiento con acceso público a blobs deshabilitado y un contenedor privado `docs`.
2. Sube un archivo `hola.txt` con `Content-Type` `text/plain`.
3. Intenta descargarlo con `curl` usando solo la URL del blob.
4. Genera una SAS de blob con permiso solo de lectura, solo HTTPS y expiración en 10 minutos; arma la URL completa.
5. Descárgalo con la URL con SAS.
6. Con la misma SAS intenta sobrescribir el blob (PUT).
7. Genera otra SAS que expire en 1 minuto, espera a que caduque y vuelve a intentar.
8. (Opcional, en tu suscripción con el rol *Storage Blob Data Contributor*) genera una *user delegation SAS* con `--as-user --auth-mode login`.
9. Borra la cuenta de almacenamiento.

## Criterios de terminado

- [ ] Sin SAS, `curl` recibe un error 4xx (404 o 409 según la configuración), no el contenido.
- [ ] Con SAS de lectura, `curl` devuelve 200 y el texto.
- [ ] El PUT con SAS de lectura devuelve 403.
- [ ] La SAS caducada devuelve 403 (`AuthenticationFailed`).
- [ ] Puedes explicar cómo revocarías una service SAS filtrada.

## Solución

> [!success]- Solución pasos 1 a 3
> ```bash
> RG=$(az group list --query "[0].name" -o tsv)
> ST=stsas$RANDOM$RANDOM
> az storage account create -n $ST -g $RG --sku Standard_LRS --allow-blob-public-access false
> KEY=$(az storage account keys list -n $ST -g $RG --query "[0].value" -o tsv)
> az storage container create -n docs --account-name $ST --account-key $KEY
> echo "hola SAS" > hola.txt
> az storage blob upload --account-name $ST --account-key $KEY -c docs -n hola.txt -f hola.txt \
>   --content-type text/plain
> URL=https://$ST.blob.core.windows.net/docs/hola.txt
> curl -i $URL
> ```

> [!success]- Solución pasos 4 a 7
> ```bash
> EXP=$(date -u -d "10 minutes" '+%Y-%m-%dT%H:%MZ')
> SAS=$(az storage blob generate-sas --account-name $ST --account-key $KEY -c docs -n hola.txt \
>   --permissions r --expiry $EXP --https-only -o tsv)
> curl -i "$URL?$SAS"                       # 200
> curl -i -X PUT -H "x-ms-blob-type: BlockBlob" --data-binary @hola.txt "$URL?$SAS"   # 403
> EXP1=$(date -u -d "1 minute" '+%Y-%m-%dT%H:%MZ')
> SAS1=$(az storage blob generate-sas --account-name $ST --account-key $KEY -c docs -n hola.txt \
>   --permissions r --expiry $EXP1 --https-only -o tsv)
> sleep 120; curl -i "$URL?$SAS1"          # 403
> ```

> [!success]- Solución: user delegation SAS y revocación
> ```bash
> az storage blob generate-sas --account-name $ST -c docs -n hola.txt --permissions r \
>   --expiry $EXP --https-only --as-user --auth-mode login -o tsv
> ```
> Revocar: una service SAS sin *stored access policy* solo se invalida **rotando la llave** con que se firmó (`az storage account keys renew`). Una user delegation SAS se invalida revocando las llaves de delegación (`az storage account revoke-delegation-keys`). Limpieza: `az storage account delete -n $ST -g $RG --yes`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
