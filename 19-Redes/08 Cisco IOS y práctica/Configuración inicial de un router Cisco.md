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
# Configuración inicial de un router Cisco

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **configuración inicial** de un router es el conjunto mínimo de pasos para dejarlo identificado, seguro, con sus interfaces activas y listo para administrarse. Un router nuevo sale de fábrica sin nombre, sin contraseñas, con las **interfaces apagadas** (`administratively down`) y sin rutas más allá de las conectadas. Tener una plantilla propia te ahorra tiempo y evita olvidos.

### Cómo funciona
Orden recomendado:
1. **Nombre** del equipo (`hostname`): aparece en el prompt y lo necesita SSH para generar llaves.
2. **Evitar búsquedas DNS** de comandos mal escritos (`no ip domain-lookup`) en laboratorio.
3. **Contraseña del modo privilegiado** con `enable secret` (guardada con hash), nunca `enable password`.
4. **Usuario local** con `username ... secret ...`.
5. **Consola** con login local, tiempo de inactividad y `logging synchronous` (para que los mensajes no corten lo que escribes).
6. **Acceso remoto** por SSH en las líneas VTY (ver [[SSH y acceso seguro a dispositivos]]).
7. **Cifrado de las contraseñas** que quedan en texto plano: `service password-encryption` (es un cifrado débil, solo evita que se lean de un vistazo).
8. **Banner** de aviso legal (`banner motd`).
9. **Interfaces**: descripción, IP, `no shutdown`.
10. **Rutas** (estática por defecto o protocolo dinámico), NTP, syslog.
11. **Guardar**: `copy running-config startup-config`.

**Estados de interfaz** en `show ip interface brief` (columnas *Status* y *Protocol*):
- `administratively down / down`: alguien aplicó `shutdown`, o es el estado de fábrica. Se corrige con `no shutdown`.
- `down / down`: activada, pero no detecta señal: cable desconectado, equipo del otro lado apagado o con `shutdown`.
- `up / down`: capa física bien, pero falla la capa 2 (encapsulación distinta, keepalives, desajuste en enlaces seriales).
- `up / up`: funcionando.

### Ejemplo
Plantilla para R1:

```text
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# no ip domain-lookup
R1(config)# enable secret <ClaveEnable>
R1(config)# username admin privilege 15 secret <ClaveAdmin>
R1(config)# service password-encryption
R1(config)# banner motd # Acceso solo para personal autorizado. Actividad monitoreada. #
R1(config)# line console 0
R1(config-line)# login local
R1(config-line)# exec-timeout 10 0
R1(config-line)# logging synchronous
R1(config-line)# exit
R1(config)# interface g0/0
R1(config-if)# description LAN Oficina VLAN 10
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# interface g0/1
R1(config-if)# description WAN Proveedor
R1(config-if)# ip address 203.0.113.2 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
R1(config)# end
R1# copy running-config startup-config
```

Verificación:

```text
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     192.168.10.1    YES manual up                    up
GigabitEthernet0/1     203.0.113.2     YES manual up                    up
GigabitEthernet0/2     unassigned      YES unset  administratively down down
R1# show interfaces description
R1# show ip route
R1# show running-config | include hostname|secret|banner
```

`show ip interface brief` es el comando de resumen por excelencia: IP, método de asignación y estado de cada interfaz en una línea. Las interfaces sin uso conviene dejarlas apagadas (y con descripción "SIN USO").

Las contraseñas de ejemplo van entre `< >` a propósito: nunca guardes las reales en tus notas ni en el repositorio de respaldos.

### Cuándo usarlo y cuándo no
Aplica esta base a cada router nuevo, en laboratorio y en producción. En producción agrega NTP, syslog, SNMPv3, AAA con RADIUS o TACACS+ y respaldos automáticos (ver [[Respaldo de configuraciones]]). Para muchos equipos, conviértela en una plantilla que despliegas con automatización. En [[Cisco Packet Tracer]] guarda esta plantilla en un archivo de texto y pégala en cada router nuevo de tus laboratorios: así practicas siempre con una base segura y te concentras en el tema del día.

### Errores comunes
- **Olvidar `no shutdown`**.
- **Usar `enable password`** en lugar de `enable secret`.
- **Confiar en `service password-encryption`** como protección real: el tipo 7 se revierte fácilmente.
- **No guardar** la configuración.

### Para profundizar
- [Cisco Guide to Harden Cisco IOS Devices](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/13608-21.html)
- [Cisco Packet Tracer en Networking Academy](https://www.netacad.com/cisco-packet-tracer)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué configuras primero en un router nuevo?
> Hostname, contraseñas (`enable secret`, consola, VTY), `no ip domain-lookup`, banner, IP y descripción en interfaces con `no shutdown`, y guardar.

> [!question]- Configura G0/0 con 192.168.1.1/24 y una descripción.
> ```
> interface g0/0
>  description LAN principal
>  ip address 192.168.1.1 255.255.255.0
>  no shutdown
> ```

> [!question]- Una interfaz aparece `administratively down`. ¿Qué significa?
> Tiene `shutdown`: falta `no shutdown`. Si aparece `down/down`, revisa el cable o el equipo del otro lado. Si es `up/down`, el problema es de capa 2 (encapsulación o keepalives).

> [!question]- ¿Qué comando muestra el resumen de interfaces con IP y estado?
> `show ip interface brief`.

## Práctica

- [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]]

