---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# EtherChannel (LACP y PAgP)

Parte de: [[19-Redes/02 Switching/_Índice Switching|Switching]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**EtherChannel** agrupa varios enlaces físicos paralelos entre dos switches (o entre un switch y un servidor o router) en **un solo enlace lógico**, llamado port-channel. Así sumas ancho de banda y redundancia: si un cable falla, el tráfico sigue por los demás sin reconvergencia de STP. Para STP el grupo es una sola interfaz, así que no bloquea ninguno de los miembros.

### Cómo funciona
Se puede formar de tres maneras:
- **LACP** (Link Aggregation Control Protocol, IEEE 802.3ad, hoy 802.1AX): estándar abierto. Modos **active** (inicia la negociación) y **passive** (solo responde).
- **PAgP** (Port Aggregation Protocol): propietario de Cisco. Modos **desirable** (inicia) y **auto** (responde).
- **Estático** (`on`): sin negociación; ambos extremos deben estar en `on`.

Combinaciones que **sí** forman canal:

| Lado A | Lado B | ¿Forma? |
|---|---|---|
| active | active | Sí |
| active | passive | Sí |
| passive | passive | No (nadie inicia) |
| desirable | desirable | Sí |
| desirable | auto | Sí |
| auto | auto | No |
| on | on | Sí |
| on | active/desirable | No (y puede causar problemas) |

Lo que **debe coincidir** en todos los puertos del grupo: velocidad y dúplex, modo (todos access en la misma VLAN o todos trunk), VLAN nativa y VLAN permitidas en trunk, y el tipo de puerto (todos capa 2 o todos capa 3). Si un puerto difiere, queda suspendido.

**Balanceo de carga**: el tráfico no se reparte trama por trama sino por **flujo**, con un hash sobre MAC, IP o puertos (`port-channel load-balance src-dst-ip`, por ejemplo). Un solo flujo grande usa un solo enlace; muchos flujos se reparten. Con LACP se pueden agrupar hasta 8 enlaces activos (y otros en espera, según la plataforma).

### Ejemplo
Uplink de 2 × 1 Gbps entre SW-ACCESO (G0/1-2) y SW-DIST (G1/0/1-2), con LACP y como trunk:

```text
SW-ACCESO(config)# interface range g0/1 - 2
SW-ACCESO(config-if-range)# channel-group 1 mode active
SW-ACCESO(config-if-range)# exit
SW-ACCESO(config)# interface port-channel 1
SW-ACCESO(config-if)# switchport mode trunk
SW-ACCESO(config-if)# switchport trunk native vlan 999
SW-ACCESO(config-if)# switchport trunk allowed vlan 10,20,99
SW-ACCESO(config-if)# end
SW-ACCESO# show etherchannel summary
Flags:  D - down   P - bundled in port-channel  s - suspended
        S - Layer2 U - in use
Group  Port-channel  Protocol    Ports
1      Po1(SU)       LACP        Gi0/1(P) Gi0/2(P)
```

Haz lo mismo en SW-DIST con `mode active` o `mode passive`. `Po1(SU)` significa capa 2 y en uso; `(P)` en cada puerto, que está agrupado. Si ves `(s)` o `(I)`, revisa qué parámetro no coincide.

Otros comandos útiles: `show etherchannel port-channel`, `show lacp neighbor`, `show interfaces port-channel 1`.

Practica en el [[Lab 2 - STP y EtherChannel en Packet Tracer]].

### Cuándo usarlo y cuándo no
Úsalo en uplinks entre switches de acceso y distribución, entre switches núcleo, hacia servidores con dos tarjetas (NIC teaming con LACP) y hacia routers en router on a stick para quitar el cuello de botella. Prefiere **LACP** por ser estándar y negociar (detecta errores de cableado). Evita `on` salvo que el otro extremo no soporte negociación.

### Errores comunes
- **Configurar los puertos físicos y el port-channel de forma distinta**: los cambios de VLAN y trunk se hacen en la interfaz port-channel.
- **Mezclar LACP en un lado y PAgP en el otro**: no forma canal.
- **Esperar que un solo flujo use 2 Gbps**: el hash lo pone en un enlace.
- **Usar `on` con negociación en el otro extremo**: posible bucle o canal caído.

### Para profundizar
- [IEEE 802.1 Working Group (802.1AX Link Aggregation)](https://www.ieee802.org/1/)
- [Cisco Catalyst 2960: guías de configuración](https://www.cisco.com/c/en/us/support/switches/catalyst-2960-series-switches/products-installation-and-configuration-guides-list.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es EtherChannel?
> Agrupa varios enlaces físicos (hasta 8 activos) en uno lógico (*port-channel*): suma ancho de banda, da redundancia y STP lo ve como un solo enlace (no bloquea ninguno).

> [!question]- ¿Qué combinaciones de modos forman el canal?
> **LACP** (estándar): active-active o active-passive. **PAgP** (Cisco): desirable-desirable o desirable-auto. **On**: solo con on del otro lado. passive-passive o auto-auto **no** forman.

> [!question]- ¿Qué debe coincidir en todos los puertos del grupo?
> Velocidad, dúplex, modo (acceso o trunk), VLAN de acceso o VLANs permitidas y VLAN nativa.

> [!question]- ¿Qué comandos crean un EtherChannel LACP trunk con Fa0/1-2?
> ```
> interface range fa0/1 - 2
>  channel-group 1 mode active
> interface port-channel 1
>  switchport mode trunk
> ```
> Verifica con `show etherchannel summary` (flags **SU** y puertos **P**).

## Práctica

- [[Lab 2 - STP y EtherChannel en Packet Tracer]]

