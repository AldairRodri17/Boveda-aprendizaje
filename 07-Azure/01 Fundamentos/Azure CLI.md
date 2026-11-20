---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Azure
tags: [azure]
---
# Azure CLI

Parte de: [[07-Azure/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **Azure CLI** (`az`) es la herramienta de línea de comandos multiplataforma para administrar Azure. Cada comando se traduce en llamadas REST a [[Azure - Resource Manager|ARM]]. Funciona igual en Windows, Linux, macOS y en **Azure Cloud Shell** (la terminal en el navegador, ya autenticada). Es ideal para tareas puntuales, scripts y pasos de pipelines en [[Azure Pipelines]] (tarea `AzureCLI@2`).

### Cómo funciona
**Estructura de un comando**: `az <grupo> [<subgrupo>] <acción> --parámetros`. Ejemplos: `az group create`, `az webapp config appsettings set`. Agrega `--help` a cualquier nivel para ver opciones y ejemplos.

**Sesión y suscripción**
```bash
az login                                  # abre el navegador (o --use-device-code)
az account list -o table                  # suscripciones disponibles
az account set --subscription "Dev-Test"  # nombre o id: todos los comandos siguientes usan esta
az account show --query name -o tsv       # confirma dónde estás antes de borrar algo
```

**Formatos de salida (`-o`)**: `json` (por defecto), `table` (para leer), `tsv` (para scripts: sin comillas, separado por tabuladores), `yaml`, `none`.

**`--query`** recibe una expresión **JMESPath** que filtra y proyecta el JSON **antes** de imprimirlo:
- `[].name` → lista de nombres
- `[?location=='eastus']` → filtro
- `[].{nombre:name, tipo:type}` → proyección con columnas propias

Con `-o tsv` y una sola propiedad obtienes valores limpios para variables de bash.

**Valores por defecto** para no repetir `--resource-group` y `--location`:
```bash
az config set defaults.group=rg-lab defaults.location=eastus
az config unset defaults.group   # para quitarlo
```

**Operaciones largas**: algunos comandos (borrar grupos, crear clústeres) esperan a que termine la operación. `--no-wait` regresa de inmediato. Los comandos destructivos piden confirmación interactiva; `--yes` (`-y`) la omite.

### Ejemplo: las tareas de la autoevaluación
```bash
#- 1. Nombres de grupos en eastus, uno por línea
az group list --query "[?location=='eastus'].name" -o tsv

#- 2. Guardar el id del grupo en una variable
RG_ID=$(az group show -n rg-lab --query id -o tsv)
echo "$RG_ID"

#- 3. Crear el grupo con etiquetas
az group create -n rg-lab -l eastus --tags ambiente=lab dueño=yo

#- 4. Recursos del grupo con nombre y tipo en tabla
az resource list -g rg-lab --query "[].{Nombre:name, Tipo:type}" -o table

#- 5. Borrar sin confirmación y sin esperar
az group delete -n rg-lab --yes --no-wait
```
Script de laboratorio seguro (bash):
```bash
#!/usr/bin/env bash
set -euo pipefail                     # detener ante el primer error
SUB="Dev-Test"; RG="rg-lab-$(date +%Y%m%d)"; LOC="eastus"
az account set --subscription "$SUB"
az group create -n "$RG" -l "$LOC" --tags ambiente=lab -o none
az appservice plan create -g "$RG" -n plan-lab --sku F1 --is-linux -o none
az webapp create -g "$RG" -p plan-lab -n "app-lab-$RANDOM" --runtime "DOTNETCORE:10.0" \
  --query defaultHostName -o tsv
echo "Al terminar: az group delete -n $RG --yes --no-wait"
```
(Lista los runtimes disponibles con `az webapp list-runtimes --os linux`.)

### Cuándo usarlo y cuándo no
- **Úsala** para explorar, consultar, automatizar tareas operativas, limpiar laboratorios y en pipelines.
- **No la uses** como la definición de tu infraestructura de producción: un script imperativo no detecta *drift* ni es idempotente por naturaleza. Para eso usa [[Bicep]] o [[Terraform]] (ver [[Azure Portal vs CLI vs IaC]]).
- En PowerShell existe el módulo **Az** ([[PowerShell para Azure]]), equivalente; elige uno por equipo.

### Errores comunes
1. **Trabajar en la suscripción equivocada**: siempre `az account show` antes de comandos destructivos.
2. **`az group delete -n rg-lab` se queda esperando** en un script: está pidiendo confirmación interactiva (y además espera a que termine). Usa `--yes` y, si no necesitas esperar, `--no-wait`.
3. **Comillas en `--query`** entre bash y PowerShell: en bash usa comillas dobles fuera y simples dentro; en PowerShell revisa el escapado o usa variables.
4. **Parsear JSON con `grep`** en lugar de `--query` + `-o tsv`.
5. **Secretos en el historial** de la terminal: pásalos por variables de entorno o desde Key Vault.

### Para profundizar
- [Documentación de la CLI de Azure](https://learn.microsoft.com/es-mx/cli/azure/)
- [Consulta de la salida de comandos con JMESPath](https://learn.microsoft.com/es-mx/cli/azure/query-azure-cli)
- [Formatos de salida de la CLI](https://learn.microsoft.com/es-mx/cli/azure/format-output-azure-cli)
- [Configuración de la CLI (az config)](https://learn.microsoft.com/es-mx/cli/azure/azure-cli-configuration)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe el comando para listar solo los **nombres** de los grupos de recursos en `eastus`, uno por línea.
> ```bash
> az group list --query "[?location=='eastus'].name" -o tsv
> ```

> [!question]- Guarda en una variable de bash el *id* del grupo `rg-lab`.
> ```bash
> RG_ID=$(az group show -n rg-lab --query id -o tsv)
> echo $RG_ID
> ```

> [!question]- Crea el grupo `rg-lab` en `eastus` con las etiquetas `ambiente=lab` y `dueño=yo`.
> ```bash
> az group create -n rg-lab -l eastus --tags ambiente=lab dueño=yo
> ```

> [!question]- Lista los recursos del grupo `rg-lab` mostrando nombre y tipo en tabla.
> ```bash
> az resource list -g rg-lab --query "[].{nombre:name, tipo:type}" -o table
> ```

> [!question]- Borra el grupo `rg-lab` sin confirmación interactiva y sin esperar a que termine.
> ```bash
> az group delete -n rg-lab --yes --no-wait
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo eliges en qué suscripción trabaja la CLI?
> `az login`, luego `az account list -o table` y `az account set --subscription "<nombre o id>"`. Verifica con `az account show`.

> [!question]- ¿Para qué sirven `--query` y `-o`?
> `--query` filtra la respuesta JSON con **JMESPath**; `-o` cambia el formato (`table`, `tsv`, `json`, `yaml`). `-o tsv` es ideal para guardar un valor en una variable de shell.

> [!question]- ¿Cómo evitas repetir `--resource-group` y `--location` en cada comando?
> Con valores por defecto: `az config set defaults.group=rg-lab defaults.location=eastus`. Ojo: después es fácil olvidar que están puestos y crear cosas en el grupo equivocado.

> [!question]- Error común en scripts: `az group delete -n rg-lab` se queda esperando. ¿Por qué?
> Pide confirmación. En scripts usa `--yes` y, si no quieres esperar, `--no-wait`.

## Práctica

- [[Lab 1 - Crear y borrar recursos con Azure CLI en el sandbox]]
- [[Lab 2 - Desplegar una API .NET a App Service]]

