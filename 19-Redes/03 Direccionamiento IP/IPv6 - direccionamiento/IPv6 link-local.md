---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# IPv6 link-local

Parte de: [[IPv6 - direccionamiento]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **dirección link-local** IPv6 es una dirección unicast válida **solo dentro del enlace local** (el segmento o VLAN). Está en el rango **`fe80::/10`** (en la práctica, `fe80::/64`). Toda interfaz con IPv6 activo tiene una, **obligatoriamente**, aunque no tenga ninguna dirección global. Es la que usan los equipos para hablar con sus vecinos inmediatos y la que usan los routers para su señalización.

### Cómo funciona
- Se genera **automáticamente** en cuanto se habilita IPv6 en la interfaz: prefijo `fe80::` + identificador de interfaz (EUI-64 en Cisco por defecto, aleatorio en la mayoría de los sistemas operativos actuales). También se puede fijar a mano.
- **No se enruta**: un router nunca reenvía un paquete con origen o destino link-local a otro enlace. Por eso la misma link-local (por ejemplo `fe80::1`) puede repetirse en cada interfaz de un router sin conflicto.
- Como la misma dirección puede existir en varias interfaces, al usarla en un host se indica la interfaz con un **identificador de zona**: `fe80::1%7` en Windows o `fe80::1%eth0` en Linux.

**Para qué la usan los routers**:
- **Router Advertisements**: el router anuncia el prefijo y se identifica con su link-local como origen.
- **Gateway predeterminado**: los hosts aprenden como gateway la **link-local del router**, no su dirección global. Si ves `fe80::1` como puerta de enlace en `ipconfig`, es correcto.
- **Siguiente salto de protocolos de enrutamiento**: OSPFv3 y EIGRP para IPv6 forman vecindades y anuncian rutas usando las link-local como origen y siguiente salto.
- **Neighbor Discovery**: Neighbor Solicitation y Advertisement entre vecinos.

### Ejemplo
Fijar una link-local fácil de recordar en las interfaces de R1 (buena práctica para que el gateway de todos sea `fe80::1`):

```text
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:acad:10::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# interface g0/1
R1(config-if)# ipv6 address 2001:db8:acad:20::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# end
R1# show ipv6 interface brief
R1# show ipv6 neighbors
```

En un host Windows:

```text
C:\> ipconfig
   Vínculo: dirección IPv6 local . . : fe80::4c1:aa2f:91d0:7e11%7
   Puerta de enlace predeterminada . : fe80::1%7
                                       192.168.10.1
C:\> ping fe80::1%7
```

Para habilitar IPv6 en una interfaz sin dirección global (solo link-local), en Cisco se usa `ipv6 enable`. Es común en enlaces entre routers con OSPFv3, donde las rutas pueden funcionar únicamente con link-local.

En IPv4 el concepto equivalente es APIPA (`169.254.0.0/16`), pero allí es un síntoma de falla; en IPv6 la link-local es parte normal del funcionamiento.

### Cuándo usarlo y cuándo no
No la configuras para dar servicio a aplicaciones; es infraestructura del enlace. Fíjala manualmente en routers (`fe80::1`, `fe80::2`…) para simplificar el diagnóstico. No la uses en DNS ni en aplicaciones para conectar entre redes: no sale del enlace.

### Errores comunes
- **Pensar que el gateway "debería" ser la global**: lo normal es la link-local.
- **Hacer ping a una link-local sin indicar la zona** en hosts con varias interfaces.
- **Bloquear `fe80::/10` en ACL o firewalls internos**: rompe NDP y el enrutamiento.
- **Esperar que se enrute**: nunca cruza un router.

### Para profundizar
- [RFC 4291: arquitectura de direccionamiento IPv6](https://www.rfc-editor.org/rfc/rfc4291)
- [RFC 4007: arquitectura de zonas IPv6 (identificador %zona)](https://www.rfc-editor.org/rfc/rfc4007)
- [RFC 4861: Neighbor Discovery](https://www.rfc-editor.org/rfc/rfc4861)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿En qué rango están las link-local?
> `fe80::/10`

> [!question]- ¿Se enrutan?
> No: solo sirven dentro del enlace (el mismo segmento).

> [!question]- ¿Para qué las usa un router?
> Como dirección de origen de los RA, como siguiente salto en las rutas y en protocolos como OSPFv3. Por eso conviene fijarla, por ejemplo `fe80::1`.

## Práctica

- [[Lab 2 - IPv6 con SLAAC en Packet Tracer]]

