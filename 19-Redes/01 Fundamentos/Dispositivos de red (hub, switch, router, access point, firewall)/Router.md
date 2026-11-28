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
# Router

Parte de: [[Dispositivos de red (hub, switch, router, access point, firewall)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **router** es un dispositivo de **capa 3** que conecta **redes distintas** y reenvía paquetes entre ellas según la dirección IP destino. Cada interfaz del router pertenece a una red diferente. Es lo que permite que tu LAN hable con otra LAN o con internet, y es también la frontera que separa dominios de broadcast.

### Cómo funciona
Cuando llega un paquete:
1. El router recibe la trama, verifica el FCS y, si la MAC destino es la suya, **desencapsula** hasta el encabezado IP.
2. Busca la IP destino en su **tabla de rutas** y elige la ruta con la **coincidencia de prefijo más larga** (la más específica). Si no hay coincidencia ni ruta por defecto, descarta el paquete y avisa con ICMP "destination unreachable".
3. Decrementa el TTL; si llega a 0, descarta y avisa con ICMP "time exceeded".
4. **Encapsula** en una trama nueva para la interfaz de salida, con su MAC como origen y la MAC del siguiente salto (resuelta con ARP) como destino.

La tabla de rutas se llena con redes **conectadas directamente** (al configurar una IP en una interfaz activa), **rutas estáticas** escritas a mano y rutas aprendidas por **protocolos dinámicos** como OSPF. El proceso de decisión se detalla en [[Cómo decide un router (tabla de rutas)]].

Por defecto, un router **no reenvía broadcasts** de capa 2 ni broadcasts IP limitados (`255.255.255.255`) de una interfaz a otra. Por eso cada interfaz es un dominio de broadcast distinto. Si necesitas que algo como DHCP cruce el router, se configura un relay (ver [[DHCP relay]]).

La **puerta de enlace predeterminada** (default gateway) de un host es la IP del router en su red local: el destino al que el host manda todo lo que no está en su propia subred.

### Ejemplo
Configuración mínima de un router con dos LAN y verificación:

```text
R1(config)# interface g0/0
R1(config-if)# description LAN Desarrollo
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# interface g0/1
R1(config-if)# description LAN Pruebas
R1(config-if)# ip address 192.168.20.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# end
R1# show ip route connected
C    192.168.10.0/24 is directly connected, GigabitEthernet0/0
C    192.168.20.0/24 is directly connected, GigabitEthernet0/1
```

Las PCs de desarrollo usan `192.168.10.1` como gateway y las de pruebas `192.168.20.1`. Ya pueden comunicarse entre sí sin rutas adicionales, porque ambas redes están directamente conectadas al mismo router.

### Cuándo usarlo y cuándo no
Usa un router para salir a internet (normalmente con NAT), unir sucursales por WAN o VPN, y separar redes. Para enrutar mucho tráfico **entre VLAN dentro del mismo edificio**, un switch capa 3 da más rendimiento por puerto que un router. En la nube, el "router" es un servicio administrado (las tablas de rutas de una VNet de Azure, por ejemplo).

### Errores comunes
- **Olvidar `no shutdown`**: en Cisco las interfaces de router vienen apagadas.
- **Poner dos interfaces en la misma subred**: IOS lo rechaza con un mensaje de traslape.
- **No configurar el gateway en los hosts** o ponerlo de otra red.
- **Pensar que basta la ruta de ida**: la respuesta también necesita una ruta de regreso.

### Para profundizar
- [RFC 1812: requisitos para routers IPv4](https://www.rfc-editor.org/rfc/rfc1812)
- [Cisco Packet Tracer en Networking Academy](https://www.netacad.com/cisco-packet-tracer)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace un router?
> Reenvía paquetes entre redes distintas según la IP destino y su tabla de rutas; también separa dominios de broadcast.

> [!question]- ¿Un router reenvía broadcasts?
> No. Por eso cada interfaz del router es un dominio de broadcast distinto (excepción configurada: `ip helper-address` para DHCP).

> [!question]- ¿Qué es la puerta de enlace predeterminada de un host?
> La IP de la interfaz del router en su red local; ahí manda todo lo que va a otras redes.

