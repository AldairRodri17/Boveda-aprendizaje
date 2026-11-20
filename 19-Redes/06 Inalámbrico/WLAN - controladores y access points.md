---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# WLAN - controladores y access points

Parte de: [[19-Redes/06 Inalámbrico/_Índice Inalámbrico|Inalámbrico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
En una red inalámbrica empresarial, los **access points** (APs) dan la cobertura de radio y un **controlador inalámbrico (WLC, Wireless LAN Controller)** los gestiona de forma centralizada: configuración, SSID, seguridad, asignación de canales y potencia, roaming de clientes y monitoreo. Este modelo "AP ligero + controlador" escala a cientos o miles de APs; para pocos APs se usan APs **autónomos** o soluciones administradas en la nube.

### Cómo funciona
**Arquitecturas**:
- **Autónoma**: cada AP tiene su propia configuración completa. Simple, pero cada cambio se repite AP por AP.
- **Basada en controlador** (*split-MAC*): el AP **ligero** (lightweight) solo maneja funciones de tiempo real de radio (beacons, ACK, cifrado); el WLC maneja autenticación, asociación, políticas, RRM (gestión de radiofrecuencia) y roaming.
- **Administrada en la nube** (por ejemplo, Cisco Meraki): el controlador vive en internet.
- **Controlador integrado** (*Embedded Wireless Controller*): uno de los APs actúa como controlador para sitios medianos.

**CAPWAP** (Control And Provisioning of Wireless Access Points) es el protocolo entre APs ligeros y WLC. Crea dos túneles UDP: **control** (puerto 5246, cifrado con DTLS) y **datos** (puerto 5247). El AP descubre al WLC por DHCP (opción 43), DNS (`CISCO-CAPWAP-CONTROLLER`), broadcast en la misma subred o configuración previa.

**Modos de reenvío del tráfico de los clientes**:
- **Local mode (centralizado)**: el tráfico de los clientes viaja dentro del túnel CAPWAP hasta el WLC y ahí sale a su VLAN. El AP solo necesita una VLAN de gestión → se conecta a un **puerto de acceso**.
- **FlexConnect**: pensado para **sucursales** conectadas por WAN a un WLC central. El control sigue yendo al WLC, pero el tráfico de los clientes puede conmutarse **localmente** en la sucursal (sin viajar por la WAN), y si se pierde el enlace al WLC, los clientes ya conectados siguen funcionando. Con switching local de varios SSID/VLAN, el puerto del AP debe ser **trunk**.

Un **AP autónomo** con varios SSID mapeados a distintas VLAN también necesita un **trunk** con esas VLAN.

**Puertos del WLC**: suele conectarse al switch con un trunk (o un EtherChannel/LAG) que lleva la VLAN de gestión y las VLAN de los clientes, que el WLC mapea a cada WLAN mediante **interfaces dinámicas**.

### Ejemplo
Oficina con 12 APs ligeros y un WLC 9800 central:

```text
! Puerto del switch hacia un AP ligero en local mode
SW1(config)# interface g1/0/10
SW1(config-if)# description AP-Piso2-Norte
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 50
SW1(config-if)# spanning-tree portfast

! Puerto hacia el WLC
SW1(config)# interface g1/0/48
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 50,10,30

! Pool DHCP de la VLAN de APs con la opción 43 apuntando al WLC
R1(config)# ip dhcp pool APS
R1(dhcp-config)# network 10.50.0.0 255.255.255.0
R1(dhcp-config)# default-router 10.50.0.1
R1(dhcp-config)# option 43 hex f104.0a63.000a
```

La opción 43 de Cisco se codifica como tipo `f1`, longitud (4 bytes por cada WLC) y la IP del WLC en hexadecimal (aquí `10.99.0.10`). En la WLAN `Itzamna-Corp` del WLC se asocia la VLAN 10 y en `Itzamna-Invitados` la VLAN 30.

En [[Cisco Packet Tracer]] existe un WLC (por ejemplo, WLC-2504) y APs ligeros (LAP-PT o 3702i) para practicar el descubrimiento y la creación de WLAN; el [[Lab 1 - Wi-Fi básico en Packet Tracer]] empieza con un AP autónomo.

### Cuándo usarlo y cuándo no
- Pocos APs (1 a 3) y una sola red: autónomos o un router con Wi-Fi.
- Muchos APs, roaming fluido, políticas centralizadas: WLC o nube.
- Sucursales con WLC central: FlexConnect.
- Varios sitios sin personal de TI: soluciones en la nube.

### Errores comunes
- **AP ligero que no encuentra al WLC**: falta opción 43, DNS o ruta; o UDP 5246/5247 bloqueado.
- **Puerto access para un AP FlexConnect** que conmuta varias VLAN localmente.
- **Olvidar PoE suficiente** para el modelo de AP: arranca con funciones limitadas o no arranca.
- **VLAN de clientes no permitida** en el trunk hacia el WLC.

### Para profundizar
- [RFC 5415: CAPWAP](https://www.rfc-editor.org/rfc/rfc5415)
- [Cisco Catalyst 9800 Wireless Controller: guías de configuración](https://www.cisco.com/c/en/us/support/wireless/catalyst-9800-series-wireless-controllers/products-installation-and-configuration-guides-list.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace un WLC?
> Centraliza la configuración y gestión de muchos APs ligeros: SSIDs, seguridad, canales, potencia y roaming.

> [!question]- ¿Qué protocolo usan los APs ligeros con el WLC?
> **CAPWAP**: túneles de control (UDP 5246) y de datos (UDP 5247) entre AP y controlador.

> [!question]- ¿Qué es FlexConnect?
> Un modo en que el AP de una sucursal puede conmutar el tráfico de los clientes localmente y seguir funcionando si pierde el enlace con el WLC central.

> [!question]- ¿Qué tipo de puerto del switch conecta un AP ligero y uno autónomo con varios SSID/VLAN?
> AP ligero en modo local: normalmente un puerto de **acceso** en la VLAN de gestión (el tráfico va en el túnel CAPWAP). AP autónomo con varias VLANs: un puerto **trunk**.

## Práctica

- [[Lab 1 - Wi-Fi básico en Packet Tracer]]

