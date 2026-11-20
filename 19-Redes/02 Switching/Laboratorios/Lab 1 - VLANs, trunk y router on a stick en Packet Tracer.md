---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Redes
herramienta: Cisco Packet Tracer
tags: [redes, laboratorio]
---
# Lab 1 - VLANs, trunk y router on a stick en Packet Tracer

Parte de: [[19-Redes/02 Switching/_Índice Switching|Switching]]

**Cubre:** [[VLANs]] · [[VLAN nativa]] · [[VLAN de administración]] · [[Trunking 802.1Q]] · [[Routing entre VLANs (router on a stick y switch capa 3)]] · [[Router on a stick]] · [[Cómo aprende un switch (tabla MAC)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear dos VLANs de usuarios en dos switches, unirlos con un trunk 802.1Q y enrutar entre VLANs con router on a stick.

## Topología

```
              R1 (2911)
               │ G0/0  (trunk)
               │ G0/1
              SW1 ──────────── SW2
            F0/1  F0/11   G0/2   G0/1   F0/1  F0/11
             │      │                    │      │
          PC-V1  PC-T1                PC-V2  PC-T2
```

| VLAN | Nombre | Red | Puerta de enlace |
|---|---|---|---|
| 10 | VENTAS | 192.168.10.0/24 | 192.168.10.1 |
| 20 | TI | 192.168.20.0/24 | 192.168.20.1 |
| 99 | GESTION (administración) | 192.168.99.0/24 | 192.168.99.1 |
| 999 | NATIVA (sin usuarios) | — | — |

## Pasos

1. Arma la topología: 1 router 2911, 2 switches 2960, 4 PCs.
2. En ambos switches crea las VLANs 10, 20, 99 y 999 con su nombre.
3. Asigna F0/1–10 a la VLAN 10 y F0/11–20 a la VLAN 20 en modo acceso.
4. Configura como trunk G0/1 de SW1 (hacia R1), G0/2 de SW1 y G0/1 de SW2 (entre switches): VLAN nativa 999, VLANs permitidas 10, 20, 99 y 999.
5. En R1 enciende G0/0 y crea las subinterfaces G0/0.10, G0/0.20 y G0/0.99.
6. Da IP de gestión a SW1 (`192.168.99.11`) y SW2 (`192.168.99.12`) en la VLAN 99, con su puerta de enlace.
7. Configura las PCs: PC-V1 `.10.10`, PC-V2 `.10.11`, PC-T1 `.20.10`, PC-T2 `.20.11`, cada una con la puerta de enlace de su VLAN.
8. Verifica y guarda la configuración en los tres equipos.
9. **Reto:** cambia la VLAN nativa solo en SW2 a 1 y observa los mensajes de CDP; luego corrígelo.

## Criterios de terminado

- [ ] `show vlan brief` muestra las VLANs 10, 20, 99 y 999 con los puertos de acceso correctos en ambos switches.
- [ ] `show interfaces trunk` muestra los trunks con VLAN nativa 999 y VLANs permitidas 10,20,99,999.
- [ ] `show ip route` en R1 muestra las tres redes como **C** en las subinterfaces.
- [ ] PC-V1 hace ping a PC-V2 (misma VLAN, distinto switch).
- [ ] PC-V1 hace ping a PC-T2 y `tracert` muestra a R1 como primer salto.
- [ ] Desde R1, `ping 192.168.99.12` responde (gestión de SW2).
- [ ] Reto: vi el aviso de *native VLAN mismatch* y lo resolví.

## Solución

> [!success]- SW1 (SW2 es igual cambiando el trunk a G0/1 y la IP a .12)
> ```
> enable
> configure terminal
> hostname SW1
> vlan 10
>  name VENTAS
> vlan 20
>  name TI
> vlan 99
>  name GESTION
> vlan 999
>  name NATIVA
> interface range f0/1 - 10
>  switchport mode access
>  switchport access vlan 10
> interface range f0/11 - 20
>  switchport mode access
>  switchport access vlan 20
> interface range g0/1 - 2
>  switchport mode trunk
>  switchport trunk native vlan 999
>  switchport trunk allowed vlan 10,20,99,999
> interface vlan 99
>  ip address 192.168.99.11 255.255.255.0
>  no shutdown
> ip default-gateway 192.168.99.1
> end
> copy running-config startup-config
> ```
> En el 2960 no hace falta `switchport trunk encapsulation dot1q` (solo soporta 802.1Q).

> [!success]- R1
> ```
> enable
> configure terminal
> hostname R1
> interface g0/0
>  no shutdown
> interface g0/0.10
>  encapsulation dot1Q 10
>  ip address 192.168.10.1 255.255.255.0
> interface g0/0.20
>  encapsulation dot1Q 20
>  ip address 192.168.20.1 255.255.255.0
> interface g0/0.99
>  encapsulation dot1Q 99
>  ip address 192.168.99.1 255.255.255.0
> end
> copy running-config startup-config
> ```

> [!success]- Reto: VLAN nativa distinta
> Con nativa 999 en SW1 y 1 en SW2, CDP muestra `%CDP-4-NATIVE_VLAN_MISMATCH`. El tráfico sin etiqueta de una VLAN cae en otra. Se corrige con `switchport trunk native vlan 999` en G0/1 de SW2.
> La VLAN 999 no tiene usuarios: así el tráfico sin etiqueta no cae en ninguna red real (mitiga *double tagging*). Si quieres que el router maneje la nativa, se usa `encapsulation dot1Q 999 native` en una subinterfaz.

> [!success]- Si algo no funciona
> - Ping entre VLANs falla pero dentro de la VLAN sí: revisa `no shutdown` en G0/0 de R1 y que G0/1 de SW1 esté en trunk.
> - PC-V2 no llega a nada: el trunk entre switches no permite la VLAN 10, o la VLAN no existe en SW2.

## Notas personales

_Qué me costó, errores que cometí, dudas._
