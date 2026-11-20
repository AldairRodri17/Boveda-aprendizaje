---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# ACL extendida

Parte de: [[ACLs (estándar y extendidas)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **ACL extendida** filtra con mucha más precisión que una estándar: puede revisar el **protocolo** (IP, TCP, UDP, ICMP, OSPF…), la **IP origen**, la **IP destino**, los **puertos** de origen y destino, flags TCP y tipos de ICMP. Se numera del **100 al 199** o del **2000 al 2699**, o se le da un nombre. Por su precisión se coloca **cerca del origen**, para descartar el tráfico no deseado antes de que recorra la red.

### Cómo funciona
Sintaxis:

```text
access-list <100-199> {permit|deny} <protocolo> <origen> <wildcard> [operador puerto]
                      <destino> <wildcard> [operador puerto] [established] [log]
```

- **Protocolo**: `ip` (todo IP), `tcp`, `udp`, `icmp`, `ospf`, `esp`, `gre`…
- **Operadores de puerto**: `eq` (igual), `neq`, `lt`, `gt`, `range`. Se pueden usar nombres: `www` (80), `443`, `domain` (53), `telnet` (23), `22`.
- **El puerto va justo después de la dirección a la que pertenece**. El puerto del servicio está en el **destino** de la petición; el del cliente en el origen (efímero).
- `established` (en TCP) coincide con paquetes que tienen ACK o RST, es decir, que pertenecen a conexiones ya iniciadas: un truco para permitir respuestas sin un firewall con estado.
- ICMP admite tipos: `echo`, `echo-reply`, `unreachable`, `time-exceeded`.

**Error de posición de puerto**: `permit tcp any eq 80 host 10.0.0.10` significa "origen con puerto **80**" hacia `10.0.0.10` con cualquier puerto. Eso coincide con **respuestas** de un servidor web, no con peticiones hacia `10.0.0.10:80`. Lo correcto para "cualquiera hacia el web de 10.0.0.10" es `permit tcp any host 10.0.0.10 eq 80`.

### Ejemplo
Servidores en `192.168.20.0/24`: la API de Itzamna.Dionisio en `192.168.20.15` (HTTPS 443) y SQL Server en `192.168.20.30` (TCP 1433). Desarrollo está en `192.168.10.0/24` (entra por G0/0.10 de R1). Requisitos:
- Desarrollo puede usar HTTPS hacia la API y SQL hacia el servidor de base de datos.
- Desarrollo puede hacer ping a toda la red de servidores.
- Nada más de Desarrollo hacia Servidores.
- Desarrollo sí puede ir a internet y al resto.

Colocada **cerca del origen**, en la entrada de la subinterfaz de Desarrollo:

```text
R1(config)# access-list 110 remark Desarrollo hacia Servidores
R1(config)# access-list 110 permit tcp 192.168.10.0 0.0.0.255 host 192.168.20.15 eq 443
R1(config)# access-list 110 permit tcp 192.168.10.0 0.0.0.255 host 192.168.20.30 eq 1433
R1(config)# access-list 110 permit icmp 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255 echo
R1(config)# access-list 110 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255 log
R1(config)# access-list 110 permit ip 192.168.10.0 0.0.0.255 any
R1(config)# interface g0/0.10
R1(config-if)# ip access-group 110 in
```

Las respuestas del ping (`echo-reply`) y de TCP vuelven desde los servidores hacia Desarrollo; esta ACL está en la **entrada** de Desarrollo, así que no las filtra.

Otras líneas típicas:

```text
! DNS: consultas UDP de cualquier host al resolver 1.1.1.1
access-list 120 permit udp any host 1.1.1.1 eq domain
! Bloquear Telnet hacia los servidores y permitir lo demás
access-list 130 deny tcp any 192.168.20.0 0.0.0.255 eq telnet
access-list 130 permit ip any any
```

Prueba desde una PC de Desarrollo: `Test-NetConnection 192.168.20.15 -Port 443` debe ser `True`; `-Port 3389` debe fallar y en el router aparecer un mensaje de log por la línea `deny ... log`.

### Cuándo usarlo y cuándo no
Úsala para controlar servicios entre segmentos, proteger servidores, permitir solo lo necesario en una DMZ y clasificar tráfico para QoS o VPN. Para seguimiento de conexiones, inspección de aplicaciones y registro detallado, complementa con un firewall stateful.

### Errores comunes
- **Puerto junto a la dirección equivocada** (origen en vez de destino).
- **Ponerla cerca del destino**: el tráfico cruza la red para ser descartado al final.
- **Olvidar permitir protocolos de control** (OSPF, DHCP, DNS) que la ACL también evalúa.
- **Usar `established` creyendo que es un firewall con estado**: solo revisa flags; no protege UDP.

### Para profundizar
- [Cisco: Configure Commonly Used IP ACLs](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/26448-ACLsamples.html)
- [IANA: registro de puertos](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe una ACL extendida que permita a 10.1.1.0/24 hacer HTTPS a 10.0.0.10 y ping a la red 10.0.0.0/24, y bloquee lo demás hacia esa red.
> ```
> access-list 120 permit tcp 10.1.1.0 0.0.0.255 host 10.0.0.10 eq 443
> access-list 120 permit icmp 10.1.1.0 0.0.0.255 10.0.0.0 0.0.0.255 echo
> access-list 120 deny ip 10.1.1.0 0.0.0.255 10.0.0.0 0.0.0.255
> access-list 120 permit ip any any
> ```
> Se aplica `in` en la interfaz del router que da a 10.1.1.0/24.

> [!question]- Bloquea Telnet desde cualquier origen hacia 10.0.0.0/24 y permite el resto.
> ```
> access-list 130 deny tcp any 10.0.0.0 0.0.0.255 eq 23
> access-list 130 permit ip any any
> ```

> [!question]- ¿Qué línea permite DNS (consultas UDP) de cualquier host hacia 8.8.8.8?
> `permit udp any host 8.8.8.8 eq 53` (o `eq domain`).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué puede revisar una ACL extendida?
> Protocolo, IP origen, IP destino y puertos de origen/destino (TCP/UDP), tipos ICMP y más. Números 100–199 y 2000–2699 (o nombre).

> [!question]- Permite que 192.168.10.0/24 solo use HTTP hacia 192.168.30.10 y bloquea el resto hacia 192.168.30.0/24, permitiendo lo demás.
> ```
> access-list 110 permit tcp 192.168.10.0 0.0.0.255 host 192.168.30.10 eq 80
> access-list 110 deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
> access-list 110 permit ip any any
> interface g0/0
>  ip access-group 110 in
> ```

> [!question]- Error común: `permit tcp any eq 80 host 10.0.0.10`. ¿Qué está mal?
> Pone el puerto 80 en el **origen**. Para tráfico hacia un servidor web el puerto va después del **destino**: `permit tcp any host 10.0.0.10 eq 80`.

## Práctica

- [[Lab 1 - ACLs estándar y extendidas en Packet Tracer]]

