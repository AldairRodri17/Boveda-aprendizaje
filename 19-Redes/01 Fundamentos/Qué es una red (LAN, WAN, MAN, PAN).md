---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Qué es una red (LAN, WAN, MAN, PAN)

Parte de: [[19-Redes/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **red de computadoras** es un conjunto de dispositivos (computadoras, teléfonos, impresoras, servidores, sensores) conectados mediante medios físicos o inalámbricos y que siguen **protocolos comunes** para compartir información y recursos. Lo que define a una red no es el tamaño sino que los equipos puedan intercambiar datos con reglas acordadas. Las redes se clasifican por alcance geográfico y por quién las administra.

### Cómo funciona
Toda red tiene tres tipos de componentes:
- **Dispositivos finales** (hosts): originan o reciben los datos; tu laptop, tu iPhone, un servidor.
- **Dispositivos intermedios**: mueven los datos; switches, routers, access points y firewalls (ver [[Dispositivos de red (hub, switch, router, access point, firewall)]]).
- **Medios**: cobre, fibra o aire (ver [[Medios físicos (cobre, fibra e inalámbrico)]]).

Clasificación por alcance:

| Tipo | Alcance | Quién la administra | Ejemplo |
|---|---|---|---|
| **PAN** (personal) | Unos metros | El usuario | iPhone con audífonos Bluetooth; el DJI Mic Mini 2 enlazado a la Osmo Pocket |
| **LAN** (local) | Casa, piso, edificio, campus | La propia organización | La red Ethernet y Wi-Fi de tu oficina |
| **MAN** (metropolitana) | Una ciudad | Un proveedor o un municipio | Anillo de fibra que une sucursales en la CDMX |
| **WAN** (amplia) | Ciudades, países | Normalmente proveedores de servicio | Enlaces MPLS o internet entre Guadalajara y Monterrey |

Además existen la **WLAN** (LAN inalámbrica), la **SAN** (red de almacenamiento) y el **campus** (varias LAN en edificios cercanos). La diferencia de fondo entre LAN y WAN no es solo la distancia: en una LAN tú controlas el equipo y el medio, con anchos de banda altos y bajo costo por Mbps; en una WAN normalmente **rentas** la conectividad a un proveedor, con menor ancho de banda por peso invertido y mayor latencia.

### Ejemplo
Tu día como desarrollador cruza todas: el Apple Watch o los audífonos usan una PAN con el iPhone; la PC con la GTX 1660 Ti se conecta a la LAN de casa por cable; la LAN sale a internet a través del módem del proveedor, que te conecta a su red metropolitana; y cuando haces `git push` a Azure DevOps, los paquetes cruzan la WAN de varios proveedores hasta un centro de datos de Microsoft.

```bash
# Ver por cuántas redes pasas para llegar a Azure DevOps (Windows)
tracert dev.azure.com
# El primer salto es tu LAN (router de casa); los siguientes, la red del proveedor y la WAN
```

**Internet** es una red de redes: miles de sistemas autónomos (proveedores, empresas, nubes) interconectados con BGP. No es una LAN gigante: nadie la administra de forma central, usa enrutamiento entre dominios y está hecha de muchas WAN y LAN de dueños distintos.

### Cuándo usarlo y cuándo no
Esta clasificación sirve para **dimensionar y elegir tecnología**: en una LAN piensas en switches, VLAN y Wi-Fi; en una WAN en enlaces de proveedor, VPN y SD-WAN (ver [[WAN y SD-WAN]]). No te obsesiones con fronteras exactas en metros: un campus grande puede parecer MAN, y lo que importa es quién administra y qué tecnología se usa.

### Errores comunes
- **"Internet es una LAN muy grande"**: es una red de redes con administración distribuida.
- **Confundir WLAN con WAN**: la W de WLAN es "wireless"; sigue siendo local.
- **Definir LAN solo por la distancia**: también cuenta el control administrativo y el tipo de tecnología.
- **Pensar que una PAN no es una red**: Bluetooth tiene sus propios protocolos y direccionamiento.

### Para profundizar
- [IEEE 802 LAN/MAN Standards Committee](https://www.ieee802.org/)
- [RFC 1958: principios de arquitectura de internet](https://www.rfc-editor.org/rfc/rfc1958)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una red de computadoras?
> Un conjunto de dispositivos (hosts y equipos intermedios) conectados por medios físicos o inalámbricos que siguen protocolos comunes para intercambiar datos y compartir recursos.

> [!question]- ¿Qué diferencia una LAN de una WAN?
> La **LAN** cubre un área pequeña (casa, oficina, edificio), la administra una sola organización y ofrece alta velocidad. La **WAN** une redes separadas geográficamente (ciudades, países) y normalmente usa enlaces de un proveedor.

> [!question]- Da un ejemplo de PAN y de MAN.
> **PAN**: tu celular conectado por Bluetooth a audífonos y reloj (pocos metros). **MAN**: la red de fibra que une los edificios de un municipio o un campus repartido en una ciudad.

> [!question]- Error común: "Internet es una LAN muy grande". ¿Por qué está mal?
> Internet es una red de redes: miles de LAN y WAN de distintos dueños interconectadas mediante routers y acuerdos entre proveedores. No tiene un único administrador ni un único dominio de broadcast.

