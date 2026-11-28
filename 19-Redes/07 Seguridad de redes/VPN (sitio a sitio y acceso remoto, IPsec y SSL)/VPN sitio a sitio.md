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
# VPN sitio a sitio

Parte de: [[VPN (sitio a sitio y acceso remoto, IPsec y SSL)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **VPN sitio a sitio** conecta **dos o más redes completas** (oficinas, sucursales, centros de datos, redes en la nube) a través de internet mediante túneles cifrados entre sus equipos de borde. Para los usuarios es **transparente**: no instalan nada; su PC envía tráfico a una IP de la otra sede y el router o firewall local lo mete en el túnel. La tecnología habitual es **IPsec**, a veces combinada con **GRE**.

### Cómo funciona
1. Se configura en cada extremo un **gateway VPN** (router o firewall) con IP pública (o detrás de NAT con NAT-T).
2. Los gateways establecen el túnel con IKEv2 y crean las SA de IPsec (ver [[IPsec]]).
3. Se define **qué tráfico va por el túnel**:
   - **VPN basada en políticas** (*policy-based*): una ACL o "crypto ACL" dice qué pares origen-destino se cifran (por ejemplo, `10.0.0.0/16 ↔ 10.1.0.0/16`). Ambos lados deben definir lo mismo en espejo.
   - **VPN basada en rutas** (*route-based*): se crea una **interfaz de túnel virtual** (VTI) y se enruta hacia ella con estáticas o con un protocolo dinámico (BGP, OSPF). Es más flexible y es la preferida hoy (Azure la usa por defecto).
4. El router recibe un paquete de `10.0.5.20` hacia `10.1.3.40`, lo encapsula y cifra hacia la IP pública del otro sitio, que lo descifra y entrega.

**GRE sobre IPsec**: IPsec puro (en su forma clásica) solo transporta tráfico **unicast IP**; no lleva **multicast** ni broadcast, así que los protocolos de enrutamiento que usan multicast (OSPF, EIGRP) no pueden formar vecindad a través de él. **GRE** es un túnel genérico que sí encapsula multicast y otros protocolos, pero **no cifra**. Combinarlos da lo mejor de ambos: GRE crea la interfaz de túnel por donde corre OSPF y pasa todo tipo de tráfico, e IPsec cifra los paquetes GRE. Hoy las VTI de IPsec también soportan protocolos de enrutamiento, y tecnologías como **DMVPN** automatizan túneles entre muchas sucursales.

### Ejemplo
Túnel GRE entre la oficina (R1) y la sucursal (R2), con OSPF por dentro (la protección IPsec se agrega con un perfil sobre la interfaz del túnel):

```text
R1(config)# interface tunnel 0
R1(config-if)# ip address 172.31.0.1 255.255.255.252
R1(config-if)# tunnel source g0/1
R1(config-if)# tunnel destination 198.51.100.20
R1(config-if)# ip mtu 1400
R1(config-if)# ip tcp adjust-mss 1360
R1(config-if)# tunnel protection ipsec profile PERFIL-SUCURSALES
R1(config-if)# exit
R1(config)# router ospf 1
R1(config-router)# network 172.31.0.0 0.0.0.3 area 0
R1(config-router)# network 10.0.0.0 0.0.255.255 area 0
R1# show interfaces tunnel 0
R1# show ip ospf neighbor
R1# show crypto ipsec sa
```

`ip mtu` y `adjust-mss` evitan problemas con paquetes grandes, porque GRE e IPsec agregan encabezados.

**El túnel sube pero no pasa tráfico de una red**: revisa
1. **Rutas**: ¿ambos lados tienen ruta hacia la red remota por el túnel? ¿Y la de regreso?
2. **Selectores de tráfico** (en VPN por políticas): ¿la red está incluida en la ACL de cifrado en **ambos** extremos, en espejo?
3. **NAT**: ¿el router está traduciendo con PAT ese tráfico antes de cifrarlo? Hay que excluir de NAT el tráfico entre sedes (una línea `deny` en la ACL de NAT).
4. **ACL o firewall** internos que bloquean esa red.
5. **Contadores** de `show crypto ipsec sa`: si `encaps` sube pero `decaps` no, el problema está del otro lado.

### Cuándo usarlo y cuándo no
Úsala para unir sucursales y la nube de forma económica. Con muchas sucursales, considera DMVPN o **SD-WAN**, que gestionan túneles y caminos automáticamente (ver [[WAN y SD-WAN]]). Si necesitas ancho de banda y latencia garantizados, contrata un enlace dedicado (MPLS, ExpressRoute) y usa la VPN como respaldo.

### Errores comunes
- **NAT antes del cifrado** sin exclusión.
- **Redes traslapadas** entre sitios.
- **MTU sin ajustar**: páginas que cargan a medias o transferencias que se cuelgan.
- **ACL de cifrado no espejada**.

### Para profundizar
- [RFC 2784: Generic Routing Encapsulation (GRE)](https://www.rfc-editor.org/rfc/rfc2784)
- [RFC 4301: arquitectura de IPsec](https://www.rfc-editor.org/rfc/rfc4301)
- [Microsoft Learn: VPN sitio a sitio con Azure VPN Gateway](https://learn.microsoft.com/azure/vpn-gateway/tutorial-site-to-site-portal)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo funciona una VPN sitio a sitio?
> Dos gateways (router o firewall) negocian un túnel IPsec; el tráfico entre las LAN definidas se cifra al salir y se descifra al llegar.

> [!question]- ¿Por qué se usa GRE sobre IPsec?
> IPsec puro no transporta multicast; GRE sí, lo que permite correr protocolos de enrutamiento (OSPF, EIGRP) a través del túnel.

> [!question]- Error común: el túnel sube pero no pasa tráfico de una red. ¿Qué revisas?
> Que la definición del tráfico interesante (ACL crypto o selectores) sea espejo en ambos lados y que NAT no esté traduciendo ese tráfico antes del túnel (falta excluirlo de NAT).

