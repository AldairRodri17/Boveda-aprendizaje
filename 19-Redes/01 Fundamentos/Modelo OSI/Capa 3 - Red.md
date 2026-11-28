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
# Capa 3 - Red

Parte de: [[Modelo OSI]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La capa de red mueve **paquetes** de un extremo a otro a través de varias redes. Aporta dos cosas que la capa 2 no tiene: un **direccionamiento lógico jerárquico** (direcciones IP con una parte de red y una de host) y el **enrutamiento**, es decir, decidir el siguiente salto para que el paquete se acerque a su destino. Los routers son los dispositivos típicos de esta capa.

### Cómo funciona
1. El host origen arma un paquete con IP origen y destino. Compara la IP destino con su propia red usando la máscara: si está en la misma red, lo envía directo; si no, lo manda a su **puerta de enlace predeterminada**.
2. Cada router que recibe el paquete lee la IP destino, busca la coincidencia más específica en su tabla de rutas (ver [[Cómo decide un router (tabla de rutas)]]), decrementa el **TTL** en 1 y reenvía por la interfaz de salida con una nueva trama de capa 2.
3. Si el TTL llega a 0, el router descarta el paquete y avisa al origen con un mensaje ICMP "Time Exceeded". Así un bucle de enrutamiento no hace circular un paquete para siempre. En IPv6 el campo equivalente se llama **Hop Limit**.
4. El destino recibe el paquete, ve que la IP es suya y entrega el contenido a la [[Capa 4 - Transporte]] según el campo **Protocol** (6 = TCP, 17 = UDP, 1 = ICMP).

Protocolos de capa 3: IPv4, IPv6, ICMP/ICMPv6 (diagnóstico y errores, ver [[ICMP, ping y traceroute]]) y, según cómo lo mires, los protocolos de enrutamiento como OSPF, que intercambian información para llenar las tablas. IPsec también opera en esta capa.

El encabezado IPv4 tiene, entre otros, estos campos: versión, longitud, DSCP (para QoS), identificación y fragmentación, TTL, protocolo, checksum del encabezado, IP origen e IP destino. El direccionamiento se estudia a fondo en [[IPv4 - estructura y clases]] y [[IPv6 - direccionamiento]].

### Ejemplo
Con `tracert` en Windows ves la capa 3 en acción: cada línea es un router que decrementó el TTL hasta cero y respondió.

```text
C:\> tracert -d 8.8.8.8
  1    1 ms    1 ms    1 ms  192.168.1.254
  2    8 ms    7 ms    9 ms  10.120.0.1
  3   12 ms   11 ms   12 ms  200.38.193.1
  ...
```

En un router Cisco, para ver cómo se decidirá el siguiente salto de un destino concreto:

```text
R1# show ip route 10.0.0.5
Routing entry for 10.0.0.0/24
  Known via "ospf 1", distance 110, metric 2
  * 10.0.12.2, from 2.2.2.2, via GigabitEthernet0/1
```

### Cuándo usarlo y cuándo no
Diagnostica en capa 3 cuando los equipos de la misma red se ven, pero no llegan a otras redes: gateway mal configurado, máscara equivocada, ruta faltante (de ida o de regreso), ACL que bloquea, o NAT ausente hacia internet. Prueba con `ping` al gateway, luego a una IP remota y usa `traceroute` para ver dónde se corta. Si `ping` a la IP funciona pero el nombre no, el problema está arriba (DNS, capa 7).

### Errores comunes
- **Olvidar la ruta de regreso**: el paquete llega, pero la respuesta no tiene cómo volver. Revisa las rutas en ambos sentidos.
- **Máscara incorrecta en un host**: el equipo cree que un destino es local y nunca lo envía al gateway.
- **Confundir TTL con tiempo**: es un contador de saltos, no de segundos (aunque el nombre diga "time").
- **Suponer que el router reenvía broadcasts**: no lo hace por defecto; por eso DHCP necesita un relay.

### Para profundizar
- [RFC 791: Internet Protocol (IPv4)](https://www.rfc-editor.org/rfc/rfc791)
- [RFC 8200: IPv6](https://www.rfc-editor.org/rfc/rfc8200)
- [RFC 792: ICMP](https://www.rfc-editor.org/rfc/rfc792)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace la capa de red?
> Direccionamiento lógico (IP) y selección de ruta entre redes distintas: lleva paquetes de extremo a extremo pasando por routers.

> [!question]- Menciona protocolos de capa 3.
> IPv4, IPv6, ICMP, y protocolos de enrutamiento como OSPF (aunque OSPF viaja sobre IP).

> [!question]- ¿Qué campo del encabezado IP evita que un paquete circule para siempre en un bucle?
> **TTL** (IPv4) o **Hop Limit** (IPv6): cada router lo resta en 1 y descarta el paquete al llegar a 0, enviando un ICMP *Time Exceeded*.

