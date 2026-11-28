---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# BGP (conceptos)

Parte de: [[19-Redes/05 Routing/_Índice Routing|Routing]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**BGP** (Border Gateway Protocol, versión 4) es el protocolo que **enruta internet**: intercambia información de alcanzabilidad entre **sistemas autónomos (AS)**, que son redes bajo una misma administración y política (un proveedor, una nube, una empresa grande), cada una identificada con un número de AS (ASN). Es un protocolo de **vector de ruta** (path vector) y su prioridad no es el camino más rápido sino aplicar **políticas**. En el CCNA basta conocer sus conceptos; también lo encontrarás en la nube (Azure VPN Gateway y ExpressRoute usan BGP).

### Cómo funciona
- Los vecinos BGP (*peers*) se configuran **manualmente** y establecen una sesión **TCP en el puerto 179**. No se descubren solos como en OSPF.
- Intercambian **prefijos** con **atributos** de ruta. El más conocido es el **AS_PATH**: la lista de AS por los que ha pasado el anuncio.
- **eBGP**: sesión entre routers de **AS distintos** (tu empresa y su proveedor). AD 20 en Cisco.
- **iBGP**: sesión entre routers del **mismo AS**, para distribuir internamente lo aprendido por eBGP. AD 200. Por la regla de *split horizon* de iBGP (lo aprendido de un peer iBGP no se reenvía a otro iBGP), se requiere malla completa o **route reflectors**.

**Evitar bucles**: al recibir un anuncio, un router eBGP revisa el AS_PATH; si encuentra **su propio número de AS**, descarta la ruta, porque significa que el anuncio ya pasó por ahí.

**Selección de la mejor ruta** (orden simplificado en Cisco):
1. Mayor **Weight** (local al router, propietario de Cisco).
2. Mayor **Local Preference** (se comparte dentro del AS; sirve para elegir por qué proveedor **salir**).
3. Ruta originada localmente.
4. **AS_PATH más corto**.
5. Menor **Origin** (IGP < EGP < incompleto).
6. Menor **MED** (sugerencia al vecino sobre por dónde **entrar**).
7. eBGP antes que iBGP, menor métrica IGP al siguiente salto, y desempates por router ID.

Para influir en el tráfico **entrante** se usa, por ejemplo, *AS_PATH prepending* (agregar tu AS varias veces para que un camino parezca más largo) o MED.

**ASN**: 16 bits (1 a 65535) o 32 bits. Los rangos privados (64512 a 65534 en 16 bits, y 4200000000 a 4294967294 en 32 bits) se usan dentro de organizaciones y en conexiones con la nube.

### Ejemplo
Empresa con AS privado 65010 conectada a un proveedor con AS 64500 (ejemplo de documentación), anunciando su bloque público:

```text
R1(config)# router bgp 65010
R1(config-router)# bgp router-id 1.1.1.1
R1(config-router)# neighbor 203.0.113.1 remote-as 64500
R1(config-router)# neighbor 203.0.113.1 description ISP-A
R1(config-router)# network 198.51.100.0 mask 255.255.255.0
R1(config-router)# end
R1# show ip bgp summary
Neighbor      V    AS MsgRcvd MsgSent  Up/Down  State/PfxRcd
203.0.113.1   4 64500    1520     980  1d02h          1
R1# show ip bgp
```

El comando `network` en BGP anuncia el prefijo **solo si existe exactamente** en la tabla de rutas (por ejemplo, con una estática a Null0). `State/PfxRcd` con un número indica sesión establecida y cuántos prefijos se recibieron; si muestra `Idle` o `Active`, la sesión no subió.

En Azure, un VPN Gateway con BGP habilitado intercambia rutas con tu router local: Azure usa el ASN 65515 por defecto para el gateway (Pendiente de confirmar en la documentación vigente antes de configurarlo).

### Cuándo usarlo y cuándo no
Usa BGP cuando te conectas a **dos o más proveedores** con tu propio bloque de direcciones (multihoming), para intercambiar rutas con la nube en VPN o enlaces dedicados, y en redes de proveedores o centros de datos grandes (BGP también se usa como IGP en fábricas spine-leaf). Para una empresa con un solo proveedor, una ruta por defecto estática es suficiente.

### Errores comunes
- **Esperar que los vecinos se descubran solos**: hay que configurar cada `neighbor`.
- **Bloquear TCP 179** en el firewall.
- **`network` con un prefijo que no está en la tabla de rutas**: no se anuncia nada.
- **Anunciar por error rutas aprendidas de un proveedor a otro**: convierte a tu empresa en tránsito (incidentes de "route leak").

### Para profundizar
- [RFC 4271: BGP-4](https://www.rfc-editor.org/rfc/rfc4271)
- [RFC 6996: ASN privados](https://www.rfc-editor.org/rfc/rfc6996)
- [Microsoft Learn: BGP con Azure VPN Gateway](https://learn.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es BGP y dónde se usa?
> El protocolo de enrutamiento entre sistemas autónomos de Internet (vector de ruta). Lo usan ISPs, nubes y empresas con varios proveedores. Viaja sobre TCP 179.

> [!question]- ¿Qué diferencia hay entre eBGP e iBGP?
> **eBGP**: entre AS distintos (AD 20). **iBGP**: dentro del mismo AS (AD 200).

> [!question]- ¿Cómo evita bucles BGP?
> Con el atributo **AS_PATH**: si un router ve su propio número de AS en la ruta, la descarta.

> [!question]- Nombra tres atributos que influyen en la mejor ruta.
> Weight (Cisco, local), Local Preference (dentro del AS, mayor gana), AS_PATH (más corto gana), MED (sugerencia al vecino, menor gana).

