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
# IPv6 multicast

Parte de: [[IPv6 - direccionamiento]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una dirección **multicast IPv6** identifica a un **grupo** de interfaces: un paquete enviado a ella llega a todos los miembros. Todas empiezan con **`ff`** (prefijo `ff00::/8`). En IPv6 el multicast tiene un papel central porque **no existe el broadcast**: todo lo que en IPv4 se hacía "a todos" (ARP, descubrimiento de routers, DHCP) se hace con grupos multicast más específicos, lo que reduce el ruido en la red.

### Cómo funciona
**Estructura**: `ff` + 4 bits de flags + 4 bits de **alcance** (scope) + 112 bits de ID de grupo. El alcance indica hasta dónde puede viajar:
- `ff01::` nodo (interfaz), `ff02::` **enlace local**, `ff05::` sitio, `ff08::` organización, `ff0e::` global.

**Direcciones bien conocidas de alcance de enlace** (no cruzan routers):

| Dirección | Grupo |
|---|---|
| `ff02::1` | Todos los nodos del enlace (lo más parecido a un broadcast) |
| `ff02::2` | Todos los routers del enlace |
| `ff02::5` y `ff02::6` | Routers OSPFv3 y DR/BDR OSPFv3 |
| `ff02::a` | Routers EIGRP |
| `ff02::1:2` | Servidores y relays DHCPv6 |
| `ff02::fb` | mDNS |

Los routers envían sus **Router Advertisements** a `ff02::1`; los hosts envían **Router Solicitations** a `ff02::2`.

**Dirección multicast de nodo solicitado** (solicited-node): `ff02::1:ff` + los **últimos 24 bits** de la dirección unicast. Cada interfaz se une automáticamente al grupo de nodo solicitado de cada una de sus direcciones unicast. Cuando un equipo quiere la MAC de un vecino (lo que en IPv4 era ARP), envía un **Neighbor Solicitation** a esa dirección, no a todos. Como solo comparten grupo las interfaces cuyas direcciones terminan en los mismos 24 bits, casi siempre la recibe un único equipo. Así se **reemplaza el broadcast de ARP**. También se usa en DAD (detección de direcciones duplicadas).

**Mapeo a Ethernet**: la MAC destino de un multicast IPv6 es `33:33` + los últimos 32 bits de la dirección. Los switches con **MLD snooping** (el equivalente de IGMP snooping) reenvían solo a los puertos interesados.

### Ejemplo
Dirección unicast `2001:db8:acad:10::25`:
- Expandida: `2001:0db8:acad:0010:0000:0000:0000:0025`
- Últimos 24 bits: `00:0025` → nodo solicitado **`ff02::1:ff00:25`**
- MAC multicast correspondiente: `33:33:ff:00:00:25`

```python
import ipaddress
ip = ipaddress.ip_address("2001:db8:acad:10::25")
b = ip.packed[-3:]
print(ipaddress.ip_address(f"ff02::1:ff{b[0]:02x}:{b[1]:02x}{b[2]:02x}"))
# ff02::1:ff00:25
```

En un router Cisco ves los grupos a los que se unió cada interfaz:

```text
R1# show ipv6 interface g0/0
  ...
  Joined group address(es):
    FF02::1
    FF02::2
    FF02::1:FF00:1
    FF02::1:FF00:25
```

`FF02::2` aparece porque el router tiene `ipv6 unicast-routing`. En [[Wireshark (captura y análisis de tráfico)]] el filtro `icmpv6.type == 135` muestra los Neighbor Solicitation hacia direcciones de nodo solicitado.

### Cuándo usarlo y cuándo no
Como administrador no "configuras" estos grupos: los usan los protocolos automáticamente. Sí debes **permitirlos** en ACL y firewalls internos (ICMPv6 hacia `ff02::/16`) y activar MLD snooping en switches con mucho multicast (video). Para aplicaciones multicast propias se usan rangos de alcance mayor y PIM en los routers.

### Errores comunes
- **Buscar un broadcast IPv6**: no existe; `ff02::1` es multicast a todos los nodos.
- **Tomar los últimos 32 bits** para el nodo solicitado: son 24.
- **Filtrar `ff02::/16` en ACL de IPv6**: rompe NDP, RA y DHCPv6.
- **Confundir alcance de enlace con global**: `ff02::` nunca sale del segmento.

### Para profundizar
- [RFC 4291: arquitectura de direccionamiento IPv6 (multicast)](https://www.rfc-editor.org/rfc/rfc4291)
- [IANA: espacio de direcciones multicast IPv6](https://www.iana.org/assignments/ipv6-multicast-addresses/ipv6-multicast-addresses.xhtml)
- [RFC 4861: Neighbor Discovery](https://www.rfc-editor.org/rfc/rfc4861)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Con qué prefijo empiezan las multicast?
> `ff00::/8`

> [!question]- ¿Qué son ff02::1 y ff02::2?
> `ff02::1`: todos los nodos del enlace. `ff02::2`: todos los routers del enlace.

> [!question]- ¿Qué es la dirección multicast de nodo solicitado y qué reemplaza?
> `ff02::1:ffXX:XXXX` (últimos 24 bits de la dirección). La usa NDP para resolver direcciones sin molestar a todos: reemplaza al broadcast de ARP.

## Práctica

- [[Lab 2 - IPv6 con SLAAC en Packet Tracer]]

