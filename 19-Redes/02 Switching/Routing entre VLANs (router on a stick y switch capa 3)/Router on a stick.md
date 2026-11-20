---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Router on a stick

Parte de: [[Routing entre VLANs (router on a stick y switch capa 3)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Router on a stick** (ROAS) es una técnica de routing entre VLANs en la que un router se conecta al switch con **un solo enlace troncal 802.1Q** y crea una **subinterfaz lógica por cada VLAN**. Cada subinterfaz tiene la IP que sirve de gateway para su VLAN. El nombre viene de que el router queda "colgado" del switch por un único cable, como un palito.

### Cómo funciona
1. El puerto del switch hacia el router se configura como **trunk** y debe permitir todas las VLAN que se van a enrutar.
2. En el router, la interfaz física (por ejemplo `G0/0`) se activa con `no shutdown` y normalmente se deja **sin IP**.
3. Se crea una subinterfaz por VLAN, `G0/0.<número>`. El número de subinterfaz es solo un nombre local; por convención se iguala al ID de VLAN para no confundirse.
4. En cada subinterfaz, `encapsulation dot1Q <vlan>` indica qué etiqueta atiende, y luego se asigna la IP del gateway.
5. Para la **VLAN nativa** (tramas sin etiqueta) se usa `encapsulation dot1Q <vlan> native`, o se pone la IP directamente en la interfaz física.

Flujo de un paquete de la VLAN 10 a la VLAN 20: la PC manda la trama al gateway; el switch la etiqueta con VLAN 10 por el trunk; el router la recibe en `G0/0.10`, enruta, y la envía por `G0/0.20` con etiqueta 20; el switch quita la etiqueta y la entrega a la PC destino. El paquete **sube y baja por el mismo cable**, por eso el enlace puede saturarse.

### Ejemplo
VLAN 10 `192.168.10.0/24`, VLAN 20 `192.168.20.0/24`, VLAN 99 nativa para gestión `192.168.99.0/24`:

```text
! Switch
SW1(config)# interface g0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 99
SW1(config-if)# switchport trunk allowed vlan 10,20,99

! Router
R1(config)# interface g0/0
R1(config-if)# no shutdown
R1(config-if)# interface g0/0.10
R1(config-subif)# description Gateway VLAN 10 Desarrollo
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
R1(config-subif)# interface g0/0.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0
R1(config-subif)# interface g0/0.99
R1(config-subif)# encapsulation dot1Q 99 native
R1(config-subif)# ip address 192.168.99.1 255.255.255.0
R1(config-subif)# end
R1# show ip interface brief
R1# show vlans
R1# show ip route connected
```

`show vlans` en el router lista cada subinterfaz con su VLAN y los paquetes que ha procesado: si una no suma paquetes, el tráfico de esa VLAN no está llegando.

Si las subinterfaces están bien y "nada funciona", revisa en este orden:
1. ¿La interfaz física tiene `no shutdown`? Si está apagada, todas las subinterfaces caen.
2. ¿El puerto del switch es trunk (`show interfaces trunk`) y permite esas VLAN?
3. ¿La VLAN nativa coincide entre router y switch?
4. ¿Las PCs tienen el gateway correcto y están en la VLAN correcta?
5. ¿El `encapsulation dot1Q` corresponde a la VLAN correcta (no confundas el número de subinterfaz con el de VLAN)?

### Cuándo usarlo y cuándo no
Útil en sucursales pequeñas, laboratorios y cuando solo tienes un router y un switch capa 2. Con mucho tráfico entre VLAN, el único enlace se vuelve cuello de botella (puedes mitigarlo con un EtherChannel hacia el router), y el router agrega latencia: ahí conviene un [[Switch capa 3 (SVI)]].

### Errores comunes
- **No activar la interfaz física**.
- **Puerto del switch en modo access** en lugar de trunk.
- **Nativa no coincidente** entre router y switch.
- **Poner IP en la física y además una subinterfaz `native`** para la misma VLAN: conflicto.

### Para profundizar
- [IEEE 802.1 Working Group (802.1Q)](https://www.ieee802.org/1/)
- [Cisco Packet Tracer en Networking Academy](https://www.netacad.com/cisco-packet-tracer)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo funciona router on a stick?
> Un enlace troncal 802.1Q entre switch y router; en el router una **subinterfaz** por VLAN, con `encapsulation dot1Q <vlan>` y la IP de puerta de enlace de esa VLAN.

> [!question]- Escribe la subinterfaz para la VLAN 10 (192.168.10.1/24) en G0/0.
> ```
> interface g0/0
>  no shutdown
> interface g0/0.10
>  encapsulation dot1Q 10
>  ip address 192.168.10.1 255.255.255.0
> ```

> [!question]- Error común: las subinterfaces están bien pero nada funciona. ¿Qué revisas?
> Que la interfaz física tenga `no shutdown`, que el puerto del switch hacia el router esté en **modo trunk** y que las VLANs estén permitidas en el trunk.

> [!question]- ¿Cómo se indica la VLAN nativa en la subinterfaz?
> `encapsulation dot1Q 99 native` en la subinterfaz de la VLAN nativa.

## Práctica

- [[Lab 1 - VLANs, trunk y router on a stick en Packet Tracer]]

