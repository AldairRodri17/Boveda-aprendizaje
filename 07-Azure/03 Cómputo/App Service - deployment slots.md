---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Azure
tags: [azure]
contenido: completo
---
# App Service - deployment slots

Parte de: [[07-Azure/03 Cómputo/_Índice Cómputo|Cómputo]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **deployment slot** es una **app viva adicional** dentro de la misma Web App de [[App Service]], con su propio nombre de host (por ejemplo `app-dionisio-prod-staging.azurewebsites.net`), su propia configuración y su propio contenido. El slot principal es `production`. Despliegas la versión nueva a un slot como `staging`, la pruebas y luego haces un **swap** (intercambio) con producción, sin tiempo de inactividad.

Requisito: plan **Standard, Premium o Isolated** (Microsoft Learn). Standard permite **5** slots por app y Premium/Isolated **20**. Los slots no tienen cargo adicional: corren en las mismas instancias del plan (y por eso compiten por sus recursos).

### Cómo funciona
```mermaid
sequenceDiagram
    participant P as Pipeline
    participant S as Slot staging
    participant Pr as Slot production
    P->>S: despliega v2
    P->>S: prueba de humo /health
    P->>Pr: swap staging ↔ production
    Note over S,Pr: App Service aplica la config de producción a staging,<br/>calienta las instancias y luego cambia el enrutamiento
    Pr-->>P: production sirve v2; staging queda con v1
```
**Qué hace el swap**
1. Aplica al slot origen los ajustes "pegajosos" del destino (y lo reinicia).
2. **Calienta** las instancias: espera respuesta HTTP en la raíz o en la ruta de `WEBSITE_SWAP_WARMUP_PING_PATH` (puedes exigir códigos con `WEBSITE_SWAP_WARMUP_PING_STATUSES`).
3. Cambia el enrutamiento: el contenido nuevo pasa a producción y el anterior queda en `staging`.

**Slot settings (sticky)**: por defecto, App Settings y Connection Strings **viajan con el contenido** en el swap. Si marcas un ajuste como **"Configuración de ranura de implementación"**, se queda **pegado al slot** y no se intercambia. Úsalo para lo que es propio del ambiente: cadena de conexión a la base de staging, `ASPNETCORE_ENVIRONMENT`, banderas que no deben llegar a producción. Algunas cosas nunca se intercambian: dominios personalizados, certificados no públicos, configuración de escalado, restricciones de IP, Always On, CORS, identidades administradas e integración con VNet, entre otras.

**Swap con vista previa (multifase)**: hace la primera fase (aplica la config de destino y reinicia), se pausa para que valides, y luego completas o cancelas. **Auto swap** intercambia solo tras cada despliegue al slot (no disponible en Linux ni en contenedores).

**Rollback**: si algo sale mal después del swap, haces **swap otra vez** entre los mismos slots: la versión anterior sigue caliente en `staging`, así que el regreso es inmediato. Por eso no despliegues nada a staging hasta confirmar que la versión nueva está bien.

### Ejemplo
```bash
RG=rg-dionisio-prod; APP=app-dionisio-prod
az webapp deployment slot create -g $RG -n $APP --slot staging --configuration-source $APP

#- Ajuste pegado al slot de staging
az webapp config appsettings set -g $RG -n $APP --slot staging \
  --slot-settings ASPNETCORE_ENVIRONMENT=Staging

#- Ruta de calentamiento usada durante el swap
az webapp config appsettings set -g $RG -n $APP --settings WEBSITE_SWAP_WARMUP_PING_PATH=/health

#- Desplegar a staging, probar, intercambiar
az webapp deploy -g $RG -n $APP --slot staging --src-path api.zip --type zip
curl -fsS https://$APP-staging.azurewebsites.net/health
az webapp deployment slot swap -g $RG -n $APP --slot staging --target-slot production

#- Rollback: el mismo comando otra vez
```
En [[Azure Pipelines]] lo típico: `AzureWebApp@1` con `deployToSlotOrASE: true` y `slotName: staging`, una prueba de humo, y `AzureAppServiceManage@0` con `Action: 'Swap Slots'` en un stage con aprobación.

### Cuándo usarlo y cuándo no
Úsalos en producción para despliegues sin caída, validación previa y rollback rápido; también para canary con "% de tráfico" hacia un slot. No los uses como ambientes de dev/test completos de larga vida: comparten recursos con producción. Cuidado con **cambios de base de datos**: el swap es instantáneo, pero la migración de esquema no se revierte con otro swap; diseña migraciones compatibles hacia atrás.

### Errores comunes
1. **Cadena de conexión no marcada como sticky**: staging termina apuntando a la base de producción o al revés.
2. **App que tarda en arrancar** sin ruta de calentamiento: los primeros usuarios tras el swap reciben latencia.
3. **Desplegar a staging justo después del swap** y perder el rollback.
4. **Esperar slots en Basic o Free**: requieren Standard o superior.
5. **Migraciones de esquema destructivas** acopladas al swap.

### Para profundizar
- [Configurar entornos de ensayo en App Service](https://learn.microsoft.com/es-mx/azure/app-service/deploy-staging-slots)
- [Procedimientos recomendados de implementación en App Service](https://learn.microsoft.com/es-mx/azure/app-service/deploy-best-practices)
- [Tarea AzureAppServiceManage@0](https://learn.microsoft.com/es-mx/azure/devops/pipelines/tasks/reference/azure-app-service-manage-v0)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un deployment slot?
> Una app «hermana» (p. ej. `staging`) con su propia URL dentro del mismo plan. Despliegas ahí, pruebas y haces **swap** con producción: Azure calienta las instancias y luego intercambia, sin tiempo de caída.

> [!question]- ¿Qué son las *slot settings* (sticky)?
> Configuraciones que **se quedan en el slot** al hacer swap (p. ej. la cadena de conexión de staging). Las no marcadas viajan con el código.

> [!question]- Algo salió mal tras el swap. ¿Cómo haces rollback?
> Otro **swap**: la versión anterior quedó en el slot `staging`. Por eso es una forma barata de blue-green. Ver [[Estrategias de despliegue (blue-green y canary)]].

> [!question]- ¿Qué nivel de plan necesitas para slots?
> Standard o superior (no Free/Basic). El número de slots depende del nivel (verificar en la página oficial).

