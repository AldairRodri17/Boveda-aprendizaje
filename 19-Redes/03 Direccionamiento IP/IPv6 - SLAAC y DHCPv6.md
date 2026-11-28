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
# IPv6 - SLAAC y DHCPv6

Parte de: [[_Índice Direccionamiento IP]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
En IPv6 un host puede obtener su configuración de tres maneras: **manual**, **SLAAC** (Stateless Address Autoconfiguration, autoconfiguración sin estado, sin servidor) o **DHCPv6** (con un servidor). SLAAC permite que el equipo construya su propia dirección global a partir del prefijo que anuncia el router. DHCPv6 puede complementar a SLAAC entregando solo datos extra (DNS) —modo **sin estado**— o asignar y registrar direcciones como DHCP en IPv4 —modo **con estado**—.

### Cómo funciona
**SLAAC paso a paso**:
1. El host habilita IPv6 y genera su **link-local** `fe80::…`; verifica con **DAD** (Duplicate Address Detection) que nadie más la use.
2. Envía un **Router Solicitation (RS)**, ICMPv6 tipo 133, a `ff02::2` (todos los routers).
3. El router responde (y además envía periódicamente) un **Router Advertisement (RA)**, tipo 134, a `ff02::1`, con: el prefijo y su longitud (debe ser **/64**), su link-local como gateway, tiempos de vida y los **flags M y O**.
4. El host toma el prefijo y le agrega un ID de interfaz de 64 bits (EUI-64 o aleatorio por privacidad), hace DAD y queda configurado. El gateway es la link-local del router.

**Flags del RA**:
- **M (Managed)** = 1: usa **DHCPv6 con estado** para obtener la dirección.
- **O (Other configuration)** = 1: usa **DHCPv6 sin estado** para obtener otros parámetros (DNS, dominio) y la dirección por SLAAC.
- Ambos en 0: solo SLAAC (los DNS pueden venir en el propio RA con la opción RDNSS, RFC 8106, que soportan los sistemas modernos).

Además, cada prefijo en el RA lleva el **flag A** (autonomous): si está en 1, el host puede autoconfigurarse con SLAAC.

| Método | Dirección | DNS | Servidor mantiene estado |
|---|---|---|---|
| SLAAC | SLAAC | RA (RDNSS) | No |
| SLAAC + DHCPv6 sin estado (O=1) | SLAAC | DHCPv6 | No |
| DHCPv6 con estado (M=1) | DHCPv6 | DHCPv6 | Sí |

Importante: en IPv6 **el gateway nunca lo da DHCPv6**; siempre se aprende del RA.

### Ejemplo
Router Cisco anunciando SLAAC con DNS por DHCPv6 sin estado en la VLAN 10:

```text
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 dhcp pool V10-SIN-ESTADO
R1(config-dhcpv6)# dns-server 2001:db8:acad:20::53
R1(config-dhcpv6)# domain-name itzamna.local
R1(config-dhcpv6)# exit
R1(config)# interface g0/0.10
R1(config-subif)# ipv6 address 2001:db8:acad:10::1/64
R1(config-subif)# ipv6 address fe80::1 link-local
R1(config-subif)# ipv6 nd other-config-flag
R1(config-subif)# ipv6 dhcp server V10-SIN-ESTADO
R1(config-subif)# end
R1# show ipv6 interface g0/0.10
  ...
  Hosts use stateless autoconfig for addresses.
  Hosts use DHCP to obtain other configuration.
```

Para DHCPv6 con estado se usa `ipv6 nd managed-config-flag`, un pool con `address prefix 2001:db8:acad:10::/64` y, para que los hosts no se autoconfiguren además por SLAAC, `ipv6 nd prefix 2001:db8:acad:10::/64 no-autoconfig`.

Sin `ipv6 unicast-routing` el router **no envía RA** ni enruta IPv6: los hosts solo tendrán link-local.

Practica en el [[Lab 2 - IPv6 con SLAAC en Packet Tracer]].

### Cuándo usarlo y cuándo no
- **SLAAC** (más RDNSS o DHCPv6 sin estado): redes de usuarios y Wi-Fi; es lo más simple y lo que soportan todos los clientes, incluido Android (que no implementa DHCPv6 con estado).
- **DHCPv6 con estado**: cuando necesitas registro centralizado de qué dirección tiene cada equipo (auditoría, integración con IPAM).
- **Manual**: routers, servidores, impresoras.

### Errores comunes
- **Prefijo distinto de /64** en la LAN: SLAAC no funciona.
- **Olvidar `ipv6 unicast-routing`**.
- **Esperar el gateway por DHCPv6**.
- **Activar M=1 y esperar que Android obtenga dirección** por DHCPv6.

### Para profundizar
- [RFC 4862: SLAAC](https://www.rfc-editor.org/rfc/rfc4862)
- [RFC 8415: DHCPv6](https://www.rfc-editor.org/rfc/rfc8415)
- [RFC 8106: opción RDNSS en Router Advertisements](https://www.rfc-editor.org/rfc/rfc8106)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo obtiene un equipo su dirección con SLAAC?
> Toma el **prefijo** del Router Advertisement (RA) que envía el router y genera él mismo el identificador de interfaz (EUI-64 o aleatorio). No hay servidor que lleve registro.

> [!question]- ¿Qué diferencia hay entre DHCPv6 sin estado y con estado?
> **Sin estado**: la dirección sale por SLAAC y DHCPv6 solo entrega datos extra (DNS). **Con estado**: el servidor DHCPv6 asigna la dirección y lleva registro.

> [!question]- ¿Qué indican los flags M y O del RA?
> M = 1 → usar DHCPv6 con estado para la dirección. O = 1 → pedir "otra" configuración (DNS) por DHCPv6.

> [!question]- ¿Qué comando necesita un router Cisco para enviar RAs y enrutar IPv6?
> `ipv6 unicast-routing`

> [!question]- ¿Qué longitud de prefijo exige SLAAC?
> /64

## Práctica

- [[Lab 2 - IPv6 con SLAAC en Packet Tracer]]

