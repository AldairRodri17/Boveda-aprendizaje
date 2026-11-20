---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# IPv6 - direccionamiento

Parte de: [[_Índice Direccionamiento IP]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**IPv6** es la versión del protocolo IP diseñada para reemplazar a IPv4. Sus direcciones miden **128 bits** (frente a 32), lo que da unos 3.4 × 10³⁸ direcciones: suficientes para que cada dispositivo tenga una dirección global sin NAT. Además simplifica el encabezado, elimina el broadcast, integra la autoconfiguración y hace obligatorio el soporte de multicast. Esta nota da el panorama; los detalles están en [[Formato y abreviación de IPv6]], [[IPv6 global unicast]], [[IPv6 link-local]] y [[IPv6 multicast]].

### Cómo funciona
**Escritura**: ocho grupos (hextetos) de 16 bits en hexadecimal separados por dos puntos, por ejemplo `2001:0db8:acad:0010:0000:0000:0000:0025`, que abreviado es `2001:db8:acad:10::25`. El prefijo se escribe como en CIDR: `/64`.

**Tres tipos de dirección**:
- **Unicast**: identifica una sola interfaz. Incluye la **global unicast (GUA)** `2000::/3`, la **link-local** `fe80::/10`, la **unique local (ULA)** `fc00::/7` (en la práctica `fd00::/8`, equivalente a las privadas), la **loopback** `::1` y la **no especificada** `::`.
- **Multicast** (`ff00::/8`): a un grupo de interfaces. Reemplaza al broadcast.
- **Anycast**: una misma dirección en varias interfaces; el paquete llega a la más cercana según el enrutamiento. Sintácticamente es igual a una unicast.

**IPv6 no tiene broadcast**. Lo que en IPv4 era un broadcast (ARP, DHCP Discover) en IPv6 es un multicast dirigido: por ejemplo, la resolución de vecinos usa la dirección de **nodo solicitado**.

**Una interfaz tiene varias direcciones IPv6**: siempre una link-local, y normalmente una o más globales (una estable y otras temporales por privacidad), más las multicast a las que se une.

**Estructura típica de una GUA con /64**: 64 bits de prefijo de red (asignado por el proveedor y la subred) y 64 bits de **identificador de interfaz**. Con /64 cada subred admite 2⁶⁴ hosts: en IPv6 no se cuidan las direcciones de host, se planean las subredes.

**Asignación de direcciones**: manual, **SLAAC** (autoconfiguración a partir de los anuncios del router), **DHCPv6** sin estado o con estado (ver [[IPv6 - SLAAC y DHCPv6]]).

### Ejemplo
Ver direcciones IPv6 en tu PC y en un router Cisco:

```text
C:\> ipconfig
   Dirección IPv6 . . . . . . . . . . : 2806:xxxx:...:1a2b   (global de tu proveedor)
   Dirección IPv6 temporal . . . . . : 2806:xxxx:...:9f3c
   Vínculo: dirección IPv6 local . . : fe80::4c1:aa2f:91d0:7e11%7
```

```text
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:acad:10::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
R1(config-if)# end
R1# show ipv6 interface brief
GigabitEthernet0/0   [up/up]
    FE80::1
    2001:DB8:ACAD:10::1
R1# show ipv6 route
```

Muchos proveedores en México ya entregan IPv6 en conexiones caseras (el prefijo `2806::/` aparece con frecuencia en redes mexicanas); revisa tu módem. Los ejemplos de esta bóveda usan `2001:db8::/32`, reservado para documentación.

### Cuándo usarlo y cuándo no
IPv6 ya es parte del examen CCNA y de la operación real: móviles, proveedores de nube y grandes sitios lo usan de forma nativa. En una empresa lo normal es **doble pila** (IPv4 e IPv6 a la vez) durante años. Plánealo con /48 por sitio y /64 por VLAN. No intentes "ahorrar" usando prefijos más largos que /64 en LAN: rompe SLAAC.

### Errores comunes
- **Pensar en IPv6 con mentalidad de IPv4**: no hay NAT por defecto ni broadcast, y no se cuentan hosts.
- **Olvidar `ipv6 unicast-routing`** en el router: no enruta ni envía Router Advertisements.
- **Bloquear ICMPv6** por completo: rompe el descubrimiento de vecinos y routers.
- **Usar /112 o /120 en VLAN de usuarios**: los clientes no se autoconfiguran.

### Para profundizar
- [RFC 4291: arquitectura de direccionamiento IPv6](https://www.rfc-editor.org/rfc/rfc4291)
- [RFC 8200: especificación de IPv6](https://www.rfc-editor.org/rfc/rfc8200)
- [LACNIC: IPv6](https://www.lacnic.net/)

## Subtemas

- [[Formato y abreviación de IPv6]]
- [[IPv6 global unicast]]
- [[IPv6 link-local]]
- [[IPv6 multicast]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuántos bits tiene IPv6 y cómo se escribe?
> 128 bits, en 8 grupos (hextetos) de 4 dígitos hexadecimales separados por `:`.

> [!question]- ¿IPv6 tiene broadcast?
> No. Usa **multicast** (y anycast).

> [!question]- ¿Cuáles son los tres tipos de dirección IPv6?
> Unicast (global, link-local, única local), multicast y anycast.

## Práctica

- [[Lab 2 - IPv6 con SLAAC en Packet Tracer]]

## Progreso de los subtemas

```base
filters:
  and:
    - file.inFolder("19-Redes/03 Direccionamiento IP/IPv6 - direccionamiento")
    - file.name != "IPv6 - direccionamiento"
views:
  - type: table
    name: Progreso
    order:
      - file.name
      - progreso
      - nivel
```
