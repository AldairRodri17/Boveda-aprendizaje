---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Azure
herramienta: Azure CLI en Cloud Shell (suscripción propia con presupuesto)
tags: [azure, laboratorio]
---
# Lab 1 - Crear y borrar recursos con Azure CLI en el sandbox

Parte de: [[07-Azure/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

**Cubre:** [[Azure CLI]] · [[Azure - suscripciones, grupos de recursos y regiones]] · [[Azure - etiquetas (tags)]] · [[Microsoft Learn sandbox (suscripción temporal sin tarjeta)]] · [[Cómo borrar recursos al terminar una práctica]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Usar la CLI en tu propia suscripción para crear un grupo de recursos desechable, proteger la práctica con un presupuesto y alerta, crear una cuenta de almacenamiento con etiquetas, consultarla con `--query` y borrar todo sin dejar nada que cobre.

> [!warning] Dónde hacerlo
> Los sandboxes de Microsoft Learn ya no están disponibles (Microsoft lo indica en la página "Run Microsoft Learn training exercises in your own subscription"). Usa tu **cuenta gratuita** de Azure (crédito inicial), **Azure for Students** o los **créditos de Visual Studio**. Todo va en un grupo de recursos solo para esta práctica, con presupuesto y alerta, y al final se borra completo. Una cuenta de almacenamiento `Standard_LRS` casi vacía cuesta centavos, pero la regla es la misma para cualquier práctica: **lo que creas, lo borras**.

## Pasos

1. Entra a Cloud Shell (bash) desde el portal, o instala Azure CLI y ejecuta `az login`. Muestra la suscripción activa en tabla y, si tienes varias, selecciona la de práctica con `az account set`.
2. Crea el grupo de recursos `rg-lab-cli` en una región cercana (p. ej. `mexicocentral` o `eastus`) con las etiquetas `ambiente=lab` y `curso=azure`, y guarda su nombre en `RG`.
3. En el portal, ve a **Cost Management > Presupuestos** con el ámbito puesto en el grupo `rg-lab-cli` y crea un presupuesto mensual pequeño (p. ej. 5 USD) con una alerta por correo al 80 % del costo real y otra al 100 % del pronosticado.
4. Guarda en `ST` un nombre único para la cuenta de almacenamiento (3–24 caracteres, solo minúsculas y números).
5. Crea la cuenta (`Standard_LRS`, `StorageV2`) en ese grupo con las etiquetas `ambiente=lab` y `curso=azure`.
6. Lista los recursos del grupo mostrando solo nombre, tipo y región en tabla.
7. Consulta solo las etiquetas de la cuenta; luego agrega la etiqueta `dueño=<tu nombre>` sin perder las demás.
8. Lista con `--query` todos los recursos del grupo que tengan `ambiente=lab`.
9. Borra la cuenta de almacenamiento y comprueba que el grupo quedó vacío.
10. Borra el grupo de recursos completo con un solo comando y confirma que ya no aparece. Si el presupuesto quedó en el ámbito del grupo, se va con él; si lo creaste en la suscripción, bórralo o déjalo como red de seguridad.

## Criterios de terminado

- [ ] `az account show -o table` muestra tu suscripción de práctica (no una de trabajo).
- [ ] El presupuesto con su alerta aparece en Cost Management con ámbito `rg-lab-cli`.
- [ ] `az resource list -g $RG -o table` muestra la cuenta con sus 3 etiquetas.
- [ ] La consulta por etiqueta devuelve solo la cuenta creada.
- [ ] Después del borrado de la cuenta, `az resource list -g $RG -o tsv` no devuelve nada.
- [ ] `az group exists -n rg-lab-cli` devuelve `false` al final.
- [ ] Escribiste en Notas personales los comandos que usarás siempre para limpiar.

## Solución

> [!success]- Solución pasos 1 a 3
> ```bash
> az account show -o table
> az account list -o table                 # si tienes varias
> az account set --subscription "<nombre o id>"
> RG=rg-lab-cli
> az group create -n $RG -l eastus --tags ambiente=lab curso=azure
> ```
> El presupuesto se crea en el portal: Cost Management > Presupuestos > Agregar, cambia el ámbito al grupo `rg-lab-cli`, monto mensual, y en "Establecer alertas" agrega el 80 % (Real) y el 100 % (Pronosticado) con tu correo. Las alertas avisan; **no detienen** los recursos, por eso el borrado final es obligatorio.

> [!success]- Solución pasos 4 y 5
> ```bash
> ST=stlab$RANDOM$RANDOM
> az storage account create -n $ST -g $RG --sku Standard_LRS --kind StorageV2 \
>   --tags ambiente=lab curso=azure
> ```

> [!success]- Solución pasos 6 a 8
> ```bash
> az resource list -g $RG --query "[].{nombre:name, tipo:type, region:location}" -o table
> az storage account show -n $ST -g $RG --query tags
> # --tags en update reemplazaría todas; 'az tag update --operation merge' agrega sin borrar
> az tag update --resource-id $(az storage account show -n $ST -g $RG --query id -o tsv) \
>   --operation merge --tags dueño=ana
> az resource list -g $RG --query "[?tags.ambiente=='lab'].name" -o tsv
> # alternativa: az resource list --tag ambiente=lab -o table
> ```

> [!success]- Solución pasos 9 y 10
> ```bash
> az storage account delete -n $ST -g $RG --yes
> az resource list -g $RG -o tsv   # vacío
>
> az group delete -n $RG --yes      # sin --no-wait para esperar a que termine
> az group exists -n $RG            # false
> az group list --tag ambiente=lab -o table   # no debe quedar nada de prácticas
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
