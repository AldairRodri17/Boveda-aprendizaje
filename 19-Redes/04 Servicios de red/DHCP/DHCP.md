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
# DHCP

Parte de: [[19-Redes/04 Servicios de red/_Índice Servicios de red|Servicios de red]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**DHCP** (Dynamic Host Configuration Protocol) asigna automáticamente a los equipos su **configuración IP**: dirección, máscara, puerta de enlace, servidores DNS, dominio y otras opciones. Sin DHCP tendrías que configurar a mano cada PC, teléfono e impresora, con el riesgo de duplicar direcciones. El servidor presta cada dirección por un tiempo (el **lease**) y la recupera si el equipo deja de usarla. Esta nota da el panorama; el intercambio de mensajes está en [[Proceso DORA de DHCP]] y el paso entre redes en [[DHCP relay]].

### Cómo funciona
- Usa **UDP**: el servidor escucha en el puerto **67** y el cliente en el **68**.
- El cliente aún no tiene IP, así que empieza con broadcast (origen `0.0.0.0`, destino `255.255.255.255`). Por eso, sin ayuda, DHCP **no cruza routers**.
- El servidor tiene uno o varios **pools** (ámbitos): un rango de direcciones por subred, más las opciones (gateway, DNS, dominio, tiempo de lease) y las **exclusiones** (direcciones que no debe repartir, como las de routers, servidores e impresoras).
- También puede hacer **reservas**: entregar siempre la misma IP a una MAC (o client-id) concreta.
- El servidor registra cada préstamo en su tabla de **bindings**. Antes de ofrecer una dirección, muchos servidores (incluido Cisco IOS) hacen un ping para comprobar que nadie la usa; si responde alguien, la marcan como **conflicto**.

Un router o switch Cisco puede actuar como servidor DHCP sencillo; en empresas suele usarse Windows Server, Infoblox, ISC Kea o el DHCP del firewall.

### Ejemplo
Pool en un router Cisco para la VLAN de desarrollo `192.168.10.0/24`, excluyendo las primeras 20 direcciones (gateway, servidores, impresoras) y la última:

```text
R1(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.20
R1(config)# ip dhcp excluded-address 192.168.10.254
R1(config)# ip dhcp pool DESARROLLO
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.1
R1(dhcp-config)# dns-server 192.168.20.53 1.1.1.1
R1(dhcp-config)# domain-name itzamna.local
R1(dhcp-config)# lease 1 12
R1(dhcp-config)# end
R1# show ip dhcp pool
R1# show ip dhcp binding
R1# show ip dhcp conflict
R1# show ip dhcp server statistics
```

`lease 1 12` significa 1 día y 12 horas. Las exclusiones se configuran **fuera** del pool, en el modo de configuración global.

En el cliente Windows:

```text
C:\> ipconfig /release
C:\> ipconfig /renew
C:\> ipconfig /all       # servidor DHCP, concesión obtenida y expiración
```

Si el cliente termina con una dirección **169.254.x.x**, no recibió respuesta de ningún servidor y se autoasignó una APIPA (ver [[Loopback y APIPA]]). Revisa en orden: enlace físico, VLAN del puerto, que el pool exista y tenga direcciones libres, que haya relay si el servidor está en otra red y que DHCP snooping no esté bloqueando.

Para que un router obtenga su propia IP por DHCP (por ejemplo, la WAN de un proveedor): `ip address dhcp` en la interfaz.

Practica en el [[Lab 1 - DHCP y NAT-PAT en Packet Tracer]].

### Cuándo usarlo y cuándo no
DHCP para todos los clientes: PCs, laptops, teléfonos, Wi-Fi, invitados. IP **estática** (o reserva) para infraestructura que otros necesitan encontrar siempre en el mismo lugar: routers, switches, servidores, impresoras, controladores. En la nube (Azure) el DHCP lo provee la plataforma; ahí defines la IP como estática o dinámica en la configuración del recurso, no dentro de la VM.

### Errores comunes
- **Olvidar las exclusiones**: el servidor reparte la IP del gateway o de un servidor.
- **Gateway equivocado en el pool**: los clientes tienen IP pero no salen de su red.
- **Pool agotado** en redes con leases largos y muchos equipos de paso (Wi-Fi de invitados): usa leases cortos.
- **Dos servidores DHCP sin coordinación** en la misma VLAN: entregas inconsistentes.

### Para profundizar
- [RFC 2131: Dynamic Host Configuration Protocol](https://www.rfc-editor.org/rfc/rfc2131)
- [RFC 2132: opciones de DHCP](https://www.rfc-editor.org/rfc/rfc2132)
- [Microsoft Learn: DHCP en Windows Server](https://learn.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)

## Subtemas

- [[Proceso DORA de DHCP]]
- [[DHCP relay]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace DHCP?
> Asigna automáticamente a los hosts IP, máscara, puerta de enlace, servidores DNS y otros parámetros por un tiempo (*lease*).

> [!question]- ¿Qué puertos usa?
> UDP **67** (servidor) y UDP **68** (cliente).

> [!question]- Configura un pool en un router Cisco para 192.168.1.0/24 excluyendo las primeras 10 direcciones.
> ```
> ip dhcp excluded-address 192.168.1.1 192.168.1.10
> ip dhcp pool LAN
>  network 192.168.1.0 255.255.255.0
>  default-router 192.168.1.1
>  dns-server 8.8.8.8
> ```
> Verifica con `show ip dhcp binding`.

> [!question]- Un equipo tiene una IP 169.254.x.x. ¿Qué significa?
> No obtuvo respuesta de un servidor DHCP y se autoasignó una dirección **APIPA**. Revisa conectividad, VLAN, pool o relay.

## Práctica

- [[Lab 1 - DHCP y NAT-PAT en Packet Tracer]]

## Progreso de los subtemas

```base
filters:
  and:
    - file.inFolder("19-Redes/04 Servicios de red/DHCP")
    - file.name != "DHCP"
views:
  - type: table
    name: Progreso
    order:
      - file.name
      - progreso
      - nivel
```
