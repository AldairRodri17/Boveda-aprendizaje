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
# IPv6 global unicast

Parte de: [[IPv6 - direccionamiento]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **dirección global unicast (GUA)** es el equivalente IPv6 de una IP pública: única en internet y enrutable globalmente. Hoy el IANA asigna GUA del rango **`2000::/3`**, es decir, direcciones que empiezan con 2 o 3 en el primer dígito hexadecimal (de `2000::` a `3fff:ffff:...`). Con GUA cada dispositivo puede ser alcanzable de extremo a extremo sin NAT (lo que no significa sin firewall).

### Cómo funciona
Una GUA típica de una LAN tiene tres partes:

| Parte | Bits típicos | Quién la define |
|---|---|---|
| **Prefijo global de enrutamiento** | 48 (a veces 56 o 60 en hogares) | El RIR y el proveedor |
| **ID de subred** | 16 (si el sitio recibe /48) | La organización |
| **ID de interfaz** | 64 | El host (manual, SLAAC, DHCPv6) |

Con un /48 por sitio, el ID de subred de 16 bits da **65,536 subredes /64**. Cada una admite 2⁶⁴ interfaces: nunca te faltarán direcciones de host, lo que se planea son las subredes.

```text
2001:0db8:acad : 0010 : 0000:0000:0000:0025
|-- prefijo /48 --|-ID subred-|---- ID de interfaz (64) ----|
```

**ID de interfaz**: se puede configurar manualmente (`::1` para el router), generar con **EUI-64** a partir de la MAC (se parte la MAC a la mitad, se inserta `fffe` y se invierte el séptimo bit), o generar aleatoriamente por privacidad (lo que hacen Windows, macOS, iOS y Android por defecto).

**El prefijo `2001:db8::/32`** está reservado para **documentación** (RFC 3849): igual que `203.0.113.0/24` en IPv4, se usa en libros, laboratorios y ejemplos para no chocar con direcciones reales. No se enruta en internet.

Otros rangos unicast que no son GUA: link-local `fe80::/10`, unique local `fc00::/7` (con `fd` en la práctica), loopback `::1`.

### Ejemplo
Plan para la oficina con prefijo de documentación `2001:db8:acad::/48`:

```text
2001:db8:acad:10::/64   VLAN 10 Desarrollo
2001:db8:acad:20::/64   VLAN 20 Servidores
2001:db8:acad:99::/64   VLAN 99 Gestión
2001:db8:acad:ff00::/64 Enlace al proveedor (o /127 en enlaces punto a punto)
```

Truco: hacer que el ID de subred coincida con el número de VLAN facilita la operación.

EUI-64 con la MAC `00:1a:2b:3c:4d:5e` en la VLAN 10:
- Partes: `001a2b` y `3c4d5e`; se inserta `fffe` → `001a2bfffe3c4d5e`.
- Se invierte el bit U/L (séptimo bit): `00` → `02`.
- ID: `021a:2bff:fe3c:4d5e` → dirección `2001:db8:acad:10:21a:2bff:fe3c:4d5e`.

```text
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0.10
R1(config-subif)# ipv6 address 2001:db8:acad:10::1/64
R1(config-subif)# interface g0/0.20
R1(config-subif)# ipv6 address 2001:db8:acad:20::/64 eui-64
R1(config-subif)# end
R1# show ipv6 interface g0/0.20 | include subnet|global
```

### Cuándo usarlo y cuándo no
Usa GUA en cualquier red con salida IPv6 a internet: tu proveedor o la nube te entregan el prefijo. Para redes solo internas puedes usar ULA (`fd00::/8`), pero muchas organizaciones prefieren GUA también dentro, con firewall, para evitar NAT. Usa siempre `/64` en LAN y documenta el plan de subredes antes de empezar.

### Errores comunes
- **Pensar que "global" significa "expuesto"**: el firewall sigue decidiendo qué entra.
- **Usar `2001:db8::/32` en producción**.
- **Subredes que no son /64 en LAN**: rompen SLAAC.
- **Confundir el ID de subred con el número de VLAN** sin documentarlo: no es obligatorio que coincidan.

### Para profundizar
- [RFC 4291: arquitectura de direccionamiento IPv6](https://www.rfc-editor.org/rfc/rfc4291)
- [RFC 3849: prefijo de documentación 2001:db8::/32](https://www.rfc-editor.org/rfc/rfc3849)
- [IANA: espacio de direcciones IPv6 unicast global](https://www.iana.org/assignments/ipv6-unicast-address-assignments/ipv6-unicast-address-assignments.xhtml)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿En qué rango están las direcciones globales unicast?
> `2000::/3` (empiezan con 2 o 3).

> [!question]- ¿Cuáles son las tres partes de una global unicast?
> Prefijo global de enrutamiento (típico /48 por sitio) · ID de subred (16 bits, hasta /64) · ID de interfaz (64 bits).

> [!question]- ¿Para qué sirve 2001:db8::/32?
> Es el rango reservado para **documentación** y ejemplos; nunca se enruta.

## Práctica

- [[Lab 2 - IPv6 con SLAAC en Packet Tracer]]

