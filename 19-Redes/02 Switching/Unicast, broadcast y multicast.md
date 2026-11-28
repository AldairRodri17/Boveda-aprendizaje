---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# Unicast, broadcast y multicast

Parte de: [[19-Redes/02 Switching/_Índice Switching|Switching]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son las tres formas de dirigir una comunicación según **cuántos destinos** la reciben:
- **Unicast**: de uno a uno. Un emisor, un receptor específico.
- **Broadcast**: de uno a todos los equipos del dominio de broadcast (la VLAN o la subred).
- **Multicast**: de uno a un **grupo** de interesados que se suscribieron.

Además existe **anycast** (de uno al más cercano de un grupo que comparte la misma dirección), muy usado en DNS públicos y en IPv6.

### Cómo funciona
| Tipo | Destino IPv4 | Destino Ethernet |
|---|---|---|
| Unicast | IP de un host (ej. `192.168.1.20`) | MAC del host o del gateway |
| Broadcast | `255.255.255.255` (limitado) o la de broadcast de la subred (ej. `192.168.1.255` en una /24) | `FF:FF:FF:FF:FF:FF` |
| Multicast | Rango `224.0.0.0/4` (224.0.0.0 a 239.255.255.255) | `01:00:5E:` + 23 bits bajos de la IP de grupo |

- Un **switch** reenvía unicast conocido por un solo puerto e inunda broadcast por toda la VLAN. Con multicast, sin más configuración, lo inunda como broadcast; con **IGMP snooping** solo lo manda a los puertos con receptores suscritos.
- Un **router** reenvía unicast según su tabla, **no** reenvía broadcast y reenvía multicast solo si tiene enrutamiento multicast (PIM).

**IPv6 no tiene broadcast**. Lo sustituye con multicast: `ff02::1` (todos los nodos del enlace), `ff02::2` (todos los routers) y la dirección de **nodo solicitado** que reemplaza a los broadcasts de ARP (ver [[IPv6 multicast]]). En Ethernet, el multicast IPv6 se mapea a MAC `33:33:` + 32 bits bajos.

Protocolos que usan multicast:
- **OSPF**: `224.0.0.5` (todos los routers OSPF) y `224.0.0.6` (DR y BDR).
- **EIGRP**: `224.0.0.10`.
- **VRRP**: `224.0.0.18`; HSRPv1 usa `224.0.0.2`.
- **mDNS** (descubrimiento de impresoras, AirPlay): `224.0.0.251`.
- Streaming de video IP en empresas y IPTV.

Protocolos que usan broadcast: ARP request, DHCP Discover y Request, NetBIOS antiguo.

### Ejemplo
Captura en [[Wireshark (captura y análisis de tráfico)]] durante un minuto en tu red de casa y aplica estos filtros:

```text
eth.dst == ff:ff:ff:ff:ff:ff          # broadcasts (ARP, DHCP)
eth.dst[0] & 1                        # cualquier trama de grupo (multicast o broadcast)
ip.dst == 224.0.0.251                 # mDNS: el iPhone buscando AirPlay o la impresora
ipv6.dst == ff02::1                   # multicast a todos los nodos IPv6
```

Verás que tu iPhone 15 Pro y otros equipos envían mDNS constantemente para descubrir servicios, y que el router anuncia su presencia IPv6 con Router Advertisements a `ff02::1`.

En un switch Cisco puedes ver los grupos multicast detectados:

```text
SW1# show ip igmp snooping groups
SW1# show interfaces counters        # columnas de unicast, multicast y broadcast
```

### Cuándo usarlo y cuándo no
- **Unicast**: casi toda la comunicación de aplicaciones (HTTP, SQL, SSH).
- **Broadcast**: solo cuando el emisor no sabe a quién preguntar (ARP, DHCP). Es caro: todos lo procesan.
- **Multicast**: cuando muchos receptores quieren el mismo flujo (video, protocolos de routing, descubrimiento). Ahorra ancho de banda frente a mandar N copias unicast.

### Errores comunes
- **Pensar que IPv6 usa broadcast**: no existe; todo es multicast.
- **Creer que multicast "cruza routers" solo**: requiere PIM y configuración.
- **Confundir broadcast de subred con el limitado**: `192.168.1.255` es el de la /24; `255.255.255.255` nunca sale del segmento.
- **Ignorar el exceso de multicast** sin IGMP snooping: se comporta como broadcast en toda la VLAN.

### Para profundizar
- [RFC 1112: multicast IP y mapeo a Ethernet](https://www.rfc-editor.org/rfc/rfc1112)
- [IANA: direcciones multicast IPv4](https://www.iana.org/assignments/multicast-addresses/multicast-addresses.xhtml)
- [RFC 4291: arquitectura de direccionamiento IPv6](https://www.rfc-editor.org/rfc/rfc4291)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Define unicast, broadcast y multicast.
> **Unicast**: de uno a uno. **Broadcast**: de uno a todos en el dominio. **Multicast**: de uno a un grupo de equipos interesados.

> [!question]- ¿Qué direcciones de destino usan en IPv4 y en Ethernet?
> Broadcast: `255.255.255.255` o la de broadcast de la subred, MAC `FF:FF:FF:FF:FF:FF`. Multicast: `224.0.0.0/4`, MAC que empieza con `01:00:5E`.

> [!question]- ¿IPv6 usa broadcast?
> **No.** Lo reemplaza con multicast (por ejemplo `FF02::1` todos los nodos).

> [!question]- Da un ejemplo de protocolo que usa multicast.
> OSPF: `224.0.0.5` (todos los routers OSPF) y `224.0.0.6` (DR/BDR).

