---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Redes
herramienta: Cisco Packet Tracer
tags: [redes, laboratorio]
contenido: completo
---
# Lab 2 - STP y EtherChannel en Packet Tracer

Parte de: [[19-Redes/02 Switching/_Índice Switching|Switching]]

**Cubre:** [[STP (Spanning Tree) y RSTP]] · [[EtherChannel (LACP y PAgP)]] · [[Port security]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Controlar la elección del root bridge con Rapid PVST+, agrupar dos enlaces en un EtherChannel LACP y proteger los puertos de acceso.

## Topología

```
        SW1 G0/1-2 ════════ G0/1-2 SW2      (EtherChannel Po1)
         F0/1 \                / F0/2
               \              /
           F0/1 \            / F0/2
                    SW3
                     │ F0/10
                    PC1
```

Equipos: 3 switches 2960 y 1 PC. Todos los enlaces entre switches serán trunk.

## Pasos

1. Arma la topología. Antes de configurar nada, ejecuta `show spanning-tree` en los tres switches y anota quién es root y qué puertos están bloqueados.
2. Activa **Rapid PVST+** en los tres switches.
3. Haz que **SW1** sea root primario y **SW2** root secundario para la VLAN 1.
4. Configura G0/1-2 de SW1 y SW2 como EtherChannel **LACP** (grupo 1) y el port-channel como trunk.
5. Configura como trunk los enlaces FastEthernet hacia SW3.
6. En SW3, F0/10 (PC1): modo acceso, PortFast, BPDU Guard y port security con máximo 1 MAC sticky, violación `shutdown`.
7. Verifica. Luego desconecta uno de los dos cables del EtherChannel y revisa el canal y STP.
8. **Reto:** conecta un segundo switch al puerto F0/10 de SW3 (en lugar de la PC) y observa qué pasa.

## Criterios de terminado

- [ ] `show spanning-tree` en SW1 dice **This bridge is the root** y el protocolo es `rstp`.
- [ ] En SW2 el root port es **Po1**; en SW3 el root port es F0/1 y F0/2 queda en rol **Altn** (bloqueado).
- [ ] `show etherchannel summary` muestra `Po1(SU)` y los dos puertos con `(P)`.
- [ ] Con un cable del canal desconectado, Po1 sigue `(SU)` con un solo puerto `(P)` y SW2 conserva Po1 como root port.
- [ ] `show port-security interface f0/10` muestra la MAC sticky de PC1.
- [ ] Reto: F0/10 queda en **err-disabled** y sé recuperarlo.

## Solución

> [!success]- Root primario y secundario
> ```
> ! SW1
> spanning-tree mode rapid-pvst
> spanning-tree vlan 1 root primary
> ! SW2
> spanning-tree mode rapid-pvst
> spanning-tree vlan 1 root secondary
> ! SW3
> spanning-tree mode rapid-pvst
> ```
> `root primary` baja la prioridad (normalmente a 24576) y `root secondary` a 28672. También puedes fijarla con `spanning-tree vlan 1 priority 4096`.

> [!success]- EtherChannel LACP (igual en SW1 y SW2)
> ```
> interface range g0/1 - 2
>  channel-group 1 mode active
> interface port-channel 1
>  switchport mode trunk
> ```
> `active` + `active` (o `active` + `passive`) forma el canal. `passive` en ambos lados no.

> [!success]- Trunks a SW3 y protección de F0/10
> ```
> ! SW1
> interface f0/1
>  switchport mode trunk
> ! SW2
> interface f0/2
>  switchport mode trunk
> ! SW3
> interface range f0/1 - 2
>  switchport mode trunk
> interface f0/10
>  switchport mode access
>  spanning-tree portfast
>  spanning-tree bpduguard enable
>  switchport port-security
>  switchport port-security maximum 1
>  switchport port-security mac-address sticky
>  switchport port-security violation shutdown
> ```

> [!success]- ¿Por qué esos roles?
> - SW2 llega al root por Po1 (dos enlaces Gigabit, costo bajo) en vez de por SW3 (dos enlaces FastEthernet, 19 + 19).
> - SW3 llega al root por F0/1 (19) en vez de por SW2 (19 + costo de Po1).
> - En el segmento SW2–SW3, SW2 tiene menor costo al root, así que su F0/2 es *designated* y la F0/2 de SW3 queda *alternate*.

> [!success]- Reto: un switch en un puerto con BPDU Guard
> El switch nuevo envía BPDUs: F0/10 pasa a **err-disabled** (`show interfaces status`). Se recupera reconectando la PC y aplicando `shutdown` y `no shutdown` en F0/10.

## Notas personales

_Qué me costó, errores que cometí, dudas._
