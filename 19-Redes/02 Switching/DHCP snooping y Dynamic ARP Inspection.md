---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# DHCP snooping y Dynamic ARP Inspection

Parte de: [[19-Redes/02 Switching/_Índice Switching|Switching]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son dos funciones de seguridad de capa 2 en switches Cisco que trabajan juntas:
- **DHCP snooping** impide que un **servidor DHCP falso** (rogue) entregue direcciones, y construye una **tabla de enlaces** (binding table) con qué IP se dio a qué MAC, en qué puerto y VLAN.
- **Dynamic ARP Inspection (DAI)** usa esa tabla para validar los mensajes ARP y bloquear **ARP spoofing**, el ataque en el que alguien se hace pasar por el gateway para interceptar tráfico (ver [[Ataques comunes de red (spoofing, MITM, DoS)]]).

### Cómo funciona
**DHCP snooping**:
1. Los puertos se clasifican como **confiables** (trusted), hacia el servidor DHCP legítimo o los uplinks, y **no confiables** (untrusted, el predeterminado), hacia los usuarios.
2. En puertos no confiables el switch **descarta mensajes de servidor** (OFFER, ACK, NAK). Así, un router doméstico conectado por error en un escritorio no puede repartir IPs.
3. El switch observa los intercambios DHCP y guarda cada asignación en la tabla de enlaces.
4. Opcionalmente limita la tasa de mensajes DHCP por puerto para evitar el agotamiento del pool (DHCP starvation).

**DAI**:
1. En puertos no confiables, el switch intercepta cada ARP y compara la pareja IP–MAC con la tabla de DHCP snooping (o con ACL ARP estáticas).
2. Si no coincide, descarta el ARP y registra el evento.
3. Los puertos hacia otros switches o routers se marcan como confiables, porque ahí la validación ya se hizo o no aplica.

DAI **depende** de DHCP snooping. Si un equipo tiene **IP estática** (un servidor, una impresora), no aparece en la tabla de enlaces y DAI bloquea sus ARP: se queda sin red. Para esos casos se crea una **ACL ARP** con la pareja IP–MAC permitida o se confía en su puerto.

### Ejemplo
VLAN 20 de usuarios; el uplink G0/1 va al switch de distribución donde está el servidor DHCP; Fa0/24 tiene una impresora con IP estática `192.168.20.250`:

```text
SW1(config)# ip dhcp snooping
SW1(config)# ip dhcp snooping vlan 20
SW1(config)# no ip dhcp snooping information option
SW1(config)# interface g0/1
SW1(config-if)# ip dhcp snooping trust
SW1(config-if)# ip arp inspection trust
SW1(config-if)# exit
SW1(config)# interface range fa0/1 - 23
SW1(config-if-range)# ip dhcp snooping limit rate 15
SW1(config-if-range)# exit
SW1(config)# arp access-list ESTATICAS-V20
SW1(config-arp-nacl)# permit ip host 192.168.20.250 mac host 0021.5a11.2233
SW1(config-arp-nacl)# exit
SW1(config)# ip arp inspection filter ESTATICAS-V20 vlan 20
SW1(config)# ip arp inspection vlan 20
SW1(config)# end
SW1# show ip dhcp snooping
SW1# show ip dhcp snooping binding
SW1# show ip arp inspection vlan 20
SW1# show ip arp inspection statistics
```

`no ip dhcp snooping information option` evita que el switch inserte la opción 82; algunos servidores DHCP (incluido el de IOS en ciertos escenarios) descartan solicitudes con esa opción si llegan por un puerto no confiable o sin relay, y los clientes no obtienen IP.

### Cuándo usarlo y cuándo no
Actívalos en switches de acceso de cualquier red con usuarios que no controlas por completo (oficinas, escuelas, coworkings). Habilita primero DHCP snooping, espera a que la tabla se llene (renovaciones de lease) y después DAI, para no cortar a nadie. En redes con muchas IP estáticas, documenta y crea las ACL ARP antes de activar DAI.

### Errores comunes
- **No marcar como confiable el uplink**: nadie recibe IP.
- **Activar DAI con servidores de IP estática sin ACL**: se quedan sin red.
- **Olvidar la opción 82** y que los clientes no reciban direcciones.
- **Confiar en todos los puertos** "para que funcione": anula la protección.

### Para profundizar
- [Cisco Catalyst 2960: guías de configuración (DHCP snooping y DAI)](https://www.cisco.com/c/en/us/support/switches/catalyst-2960-series-switches/products-installation-and-configuration-guides-list.html)
- [RFC 3046: opción 82 de DHCP (Relay Agent Information)](https://www.rfc-editor.org/rfc/rfc3046)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace DHCP snooping?
> Separa puertos **confiables** (hacia el servidor DHCP real) de no confiables: bloquea ofertas DHCP de servidores falsos y crea una tabla de asignaciones IP-MAC-puerto.

> [!question]- ¿Qué hace Dynamic ARP Inspection (DAI)?
> Revisa los mensajes ARP en puertos no confiables contra la tabla de DHCP snooping y descarta los falsos; mitiga ARP spoofing y ataques MITM.

> [!question]- Comandos mínimos para la VLAN 10 con uplink G0/1 confiable.
> ```
> ip dhcp snooping
> ip dhcp snooping vlan 10
> ip arp inspection vlan 10
> interface g0/1
>  ip dhcp snooping trust
>  ip arp inspection trust
> ```

> [!question]- Error común: activas DAI y un servidor con IP estática se queda sin red. ¿Por qué?
> No aparece en la tabla de snooping (no usó DHCP). Necesita una entrada estática (ARP ACL o binding) o que su puerto sea confiable.

