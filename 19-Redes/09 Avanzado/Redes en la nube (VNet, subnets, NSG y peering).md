---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Redes en la nube (VNet, subnets, NSG y peering)

Parte de: [[19-Redes/09 Avanzado/_Índice Avanzado|Avanzado]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
En Azure, la red privada donde viven tus recursos (VM, App Service con integración de red, bases de datos con Private Endpoint, AKS) es la **Virtual Network (VNet)**. Una VNet tiene uno o varios **espacios de direcciones** en notación CIDR y se divide en **subnets**. El tráfico se filtra con **Network Security Groups (NSG)** y las VNet se conectan entre sí con **peering** o con la red local mediante VPN o ExpressRoute. Es exactamente lo que aprendes para el CCNA, pero definido por software (ver [[SDN (redes definidas por software)]]).

### Cómo funciona
**VNet y subnets**:
- La VNet existe en una **región** y una suscripción; su espacio de direcciones suele ser privado RFC 1918 (por ejemplo, `10.50.0.0/16`).
- Las **subnets** dividen ese espacio (`10.50.1.0/24` para la API, `10.50.2.0/24` para datos). Cada recurso con tarjeta de red toma una IP de su subnet.
- Planea el espacio con [[VLSM]] y para que **no se traslape** con tu red local ni con otras VNet con las que harás peering: dos redes traslapadas no se pueden conectar.
- **Azure reserva 5 IPs en cada subnet**: la primera (dirección de red), las tres siguientes (gateway predeterminado y DNS de Azure) y la última (broadcast). En una `/24` quedan 251 utilizables; en una `/29`, solo 3. La subnet más pequeña permitida es `/29`.
- El enrutamiento entre subnets de la misma VNet es automático (rutas del sistema). Con **tablas de rutas definidas por el usuario (UDR)** puedes forzar el tráfico a través de un firewall.

**NSG**:
- Es un conjunto de reglas **con estado** (si permites una conexión de entrada, la respuesta sale sin regla adicional) que se asocia a una **subnet**, a una **tarjeta de red**, o a ambas.
- Cada regla tiene **prioridad** (de 100 a 4096), origen, destino, puerto, protocolo, dirección (entrada o salida) y acción.
- Se evalúan por **prioridad ascendente** (el número más bajo primero) y la **primera coincidencia** decide; no se siguen evaluando.
- Hay reglas predeterminadas con prioridades altas (65000 y siguientes): permitir tráfico dentro de la VNet, permitir el balanceador de Azure y **denegar todo lo demás** de entrada; de salida, permitir la VNet e internet y denegar el resto.
- Puedes usar **etiquetas de servicio** (`Internet`, `VirtualNetwork`, `Sql`, `AzureLoadBalancer`) y **grupos de seguridad de aplicaciones** en lugar de IPs.

**Peering**:
- Conecta dos VNet (en la misma o en distinta región) por la red troncal de Microsoft, con baja latencia; los recursos se comunican con IP privadas.
- **No es transitivo**: si A está emparejada con B y B con C, **A no llega a C** automáticamente. Se resuelve con peering directo A–C o con una topología **hub-and-spoke** en la que el hub tiene un firewall o NVA y UDR que reenvían, o con Azure Virtual WAN.
- Con *gateway transit*, las VNet spoke pueden usar el VPN Gateway del hub para llegar a la red local.

### Ejemplo
Red para Itzamna.Dionisio y su base de datos:

```bash
az network vnet create -g rg-itzamna -n vnet-prod --address-prefixes 10.50.0.0/16 \
  --subnet-name snet-api --subnet-prefixes 10.50.1.0/24
az network vnet subnet create -g rg-itzamna --vnet-name vnet-prod -n snet-datos \
  --address-prefixes 10.50.2.0/24
az network nsg create -g rg-itzamna -n nsg-datos
az network nsg rule create -g rg-itzamna --nsg-name nsg-datos -n Permitir-SQL-desde-API \
  --priority 100 --direction Inbound --access Allow --protocol Tcp \
  --source-address-prefixes 10.50.1.0/24 --destination-port-ranges 1433
az network nsg rule create -g rg-itzamna --nsg-name nsg-datos -n Denegar-resto-VNet \
  --priority 200 --direction Inbound --access Deny --protocol '*' \
  --source-address-prefixes VirtualNetwork --destination-port-ranges '*'
az network vnet subnet update -g rg-itzamna --vnet-name vnet-prod -n snet-datos \
  --network-security-group nsg-datos
```

La regla 100 deja entrar SQL solo desde la subnet de la API; la 200 bloquea cualquier otro origen de la VNet, que la regla predeterminada `AllowVnetInBound` (65000) habría permitido. Guarda este script en tu repositorio y ejecútalo desde un pipeline de Azure DevOps, o mejor exprésalo en Bicep o Terraform.

Para diagnosticar, **Network Watcher** ofrece *IP flow verify* (qué regla de NSG permite o bloquea un flujo) y *Next hop* (qué ruta se usa), equivalentes en la nube a revisar una ACL y `show ip route`.

### Cuándo usarlo y cuándo no
Usa VNet con subnets por capa (frontend, API, datos, gestión) y NSG en cada subnet para cualquier carga en Azure que necesite red privada. Usa peering para conectar VNet de la organización y hub-and-spoke cuando sean varias. Si tus servicios son solo PaaS públicos sin datos sensibles, quizá no necesites una VNet, aunque para producción conviene usar Private Endpoints.

### Errores comunes
- **Espacios de direcciones traslapados** con la red local u otras VNet.
- **Subnets demasiado pequeñas**: olvidar las 5 IPs reservadas o el crecimiento (AKS consume muchas IPs).
- **Esperar que el peering sea transitivo**.
- **Reglas de NSG con prioridad mal ordenada**: un deny de prioridad 100 anula un allow de 200.

### Para profundizar
- [Microsoft Learn: ¿Qué es Azure Virtual Network?](https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview)
- [Microsoft Learn: grupos de seguridad de red](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview)
- [Microsoft Learn: emparejamiento de redes virtuales](https://learn.microsoft.com/azure/virtual-network/virtual-network-peering-overview)
- [Microsoft Learn: preguntas frecuentes de Azure Virtual Network (IPs reservadas)](https://learn.microsoft.com/azure/virtual-network/virtual-networks-faq)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una VNet y cómo se relaciona con subnets?
> Una red privada aislada en Azure con uno o más espacios de direcciones; se divide en **subnets** donde viven los recursos (VMs, servicios).

> [!question]- ¿Cuántas IPs reserva Azure en cada subnet?
> **5**: las primeras cuatro y la última. Una /29 deja solo 3 utilizables.

> [!question]- ¿Cómo se evalúan las reglas de un NSG?
> Por **prioridad** (número menor primero, de 100 a 4096); la primera que coincide gana. Hay reglas predeterminadas al final que permiten tráfico dentro de la VNet y niegan el resto de entrada.

> [!question]- ¿El peering de VNet es transitivo?
> **No.** Si A está emparejada con B y B con C, A no llega a C sin peering directo o un hub con enrutamiento (firewall o gateway).

