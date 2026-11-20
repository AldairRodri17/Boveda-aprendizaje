---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Configuración inicial de un switch Cisco

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **configuración inicial** de un switch Cisco deja el equipo identificado, con acceso seguro, una **IP de gestión** para administrarlo de forma remota y sus puertos organizados. A diferencia de un router, un switch de capa 2 funciona "de fábrica" en cuanto lo conectas (todos los puertos en la VLAN 1, aprendiendo MAC), pero así queda inseguro y sin forma de administrarlo en remoto.

### Cómo funciona
Pasos básicos:
1. **Nombre**, `no ip domain-lookup`, `enable secret`, usuario local, consola y banner (igual que en un router; ver [[Configuración inicial de un router Cisco]]).
2. **VLAN** necesarias, con nombre, y asignación de puertos de acceso (ver [[VLANs]]).
3. **IP de gestión** en una SVI (`interface vlan N`) y **`ip default-gateway`** para responder desde otras redes (ver [[VLAN de administración]]).
4. **SSH** en las VTY (ver [[SSH y acceso seguro a dispositivos]]).
5. **Puertos troncales** hacia otros switches o routers, con VLAN permitidas y nativa definida.
6. **Seguridad de puertos de acceso**: modo access, `nonegotiate`, PortFast y BPDU Guard, port security.
7. **Puertos sin uso** apagados y en una VLAN sin uso.
8. NTP, syslog y guardar.

**¿Por qué los puertos del switch funcionan sin `no shutdown` y los del router no?** Por diseño: un switch existe para conectar equipos al instante, así que sus puertos vienen **habilitados** (`no shutdown`) en la VLAN 1. Un router conecta redes distintas y una interfaz activa con una IP equivocada puede causar problemas de enrutamiento, así que Cisco las entrega **apagadas** para que el administrador las active a propósito. La SVI `interface vlan 1` del switch, en cambio, sí viene apagada.

En switches de capa 2, la IP de gestión **no** se pone en un puerto físico (no aceptan `ip address` en un puerto de capa 2): va en la SVI.

### Ejemplo
Configuración base de SW1 con gestión en la VLAN 1 (aceptable en un laboratorio; en producción usa una VLAN dedicada):

```text
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1
SW1(config)# no ip domain-lookup
SW1(config)# enable secret <ClaveEnable>
SW1(config)# username admin privilege 15 secret <ClaveAdmin>
SW1(config)# line console 0
SW1(config-line)# login local
SW1(config-line)# logging synchronous
SW1(config-line)# exit
SW1(config)# interface vlan 1
SW1(config-if)# ip address 192.168.1.2 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit
SW1(config)# ip default-gateway 192.168.1.1
SW1(config)# interface range fa0/1 - 22
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# spanning-tree portfast
SW1(config-if-range)# spanning-tree bpduguard enable
SW1(config-if-range)# interface range fa0/23 - 24
SW1(config-if-range)# description SIN USO
SW1(config-if-range)# shutdown
SW1(config-if-range)# interface g0/1
SW1(config-if)# description Uplink a R1
SW1(config-if)# end
SW1# copy running-config startup-config
```

Verificación:

```text
SW1# show ip interface brief | include Vlan1
Vlan1      192.168.1.2   YES manual up   up
SW1# show interfaces status
SW1# show vlan brief
SW1# show running-config | include default-gateway
```

Desde una PC en la misma red: `ping 192.168.1.2` debe responder. Desde otra red, solo si el gateway está configurado y el router tiene ruta de regreso.

En switches con plataforma IOS XE más recientes, los puertos se llaman `GigabitEthernet1/0/1` y la configuración es muy similar.

### Cuándo usarlo y cuándo no
Aplica esta base a todo switch administrable antes de ponerlo en producción. Un switch no administrable (de escritorio) no se configura, pero tampoco ofrece VLAN, seguridad ni monitoreo: en oficinas, evita usarlos salvo para extender un escritorio puntual.

### Errores comunes
- **IP de gestión sin `ip default-gateway`**.
- **Dejar todos los puertos en VLAN 1 y activos** sin seguridad.
- **PortFast en uplinks** hacia otros switches.
- **Olvidar que la base de VLAN está en `vlan.dat`**: al borrar solo la startup-config, las VLAN siguen ahí.

### Para profundizar
- [Cisco Catalyst 2960: guías de configuración](https://www.cisco.com/c/en/us/support/switches/catalyst-2960-series-switches/products-installation-and-configuration-guides-list.html)
- [Cisco Guide to Harden Cisco IOS Devices](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/13608-21.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué pasos básicos lleva la configuración inicial de un switch?
> Hostname, `enable secret`, contraseña de consola, `service password-encryption`, banner, SVI de gestión con IP, `ip default-gateway`, acceso remoto (SSH), apagar puertos sin uso y guardar.

> [!question]- Escribe la configuración mínima de gestión en la VLAN 1.
> ```
> hostname SW1
> enable secret Cl4ss!
> line console 0
>  password Cons0la
>  login
> interface vlan 1
>  ip address 192.168.1.2 255.255.255.0
>  no shutdown
> ip default-gateway 192.168.1.1
> ```

> [!question]- ¿Por qué los puertos del switch funcionan sin `no shutdown` y los del router no?
> En un switch los puertos vienen **encendidos** por defecto; en un router las interfaces vienen **apagadas**. Buena práctica: apagar en el switch los puertos sin uso.

## Práctica

- [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]]

