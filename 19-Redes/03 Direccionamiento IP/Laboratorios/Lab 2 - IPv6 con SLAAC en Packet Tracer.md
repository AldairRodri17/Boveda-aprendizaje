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
# Lab 2 - IPv6 con SLAAC en Packet Tracer

Parte de: [[_Índice Direccionamiento IP]]

**Cubre:** [[IPv6 - direccionamiento]] · [[Formato y abreviación de IPv6]] · [[IPv6 global unicast]] · [[IPv6 link-local]] · [[IPv6 multicast]] · [[IPv6 - SLAAC y DHCPv6]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tu `.pkt` en esta carpeta.

## Objetivo

Activar IPv6 en un router, hacer que las PCs se configuren solas con SLAAC y comprobar la comunicación entre dos redes IPv6.

## Topología

La misma del [[Lab 1 - Subredes IPv4 en Packet Tracer]] (puedes partir de tu archivo): R1 con G0/0 hacia la LAN A y G0/1 hacia la LAN B.

## Pasos

1. Usa estos prefijos de documentación: LAN A `2001:db8:acad:1::/64`, LAN B `2001:db8:acad:2::/64`.
2. En R1 habilita el enrutamiento IPv6 y asigna en cada interfaz la dirección `::1` del prefijo y la link-local fija `fe80::1`.
3. En cada PC cambia IPv6 Configuration a **Automatic** (SLAAC).
4. Verifica con los comandos de los criterios.
5. **Reto:** escribe la dirección completa (sin abreviar) que obtuvo PC-A1 y luego abrévala.

## Criterios de terminado

- [ ] `show ipv6 interface brief` en R1 muestra las dos direcciones global unicast y la link-local `FE80::1`.
- [ ] Cada PC obtuvo una dirección del prefijo correcto **sin escribirla a mano**, y su puerta de enlace es `FE80::1`.
- [ ] PC-A1 hace `ping` a la dirección IPv6 de PC-B1.
- [ ] `show ipv6 route` muestra las dos redes como conectadas (C) y las direcciones locales (L).
- [ ] Puedo explicar por qué la puerta de enlace es una link-local y no la `2001:db8:acad:1::1`.
- [ ] Reto: expandí y abrevié correctamente la dirección de PC-A1.

## Solución

> [!success]- Configuración de R1
> ```
> enable
> configure terminal
> ipv6 unicast-routing
> interface g0/0
>  ipv6 address 2001:db8:acad:1::1/64
>  ipv6 address fe80::1 link-local
>  no shutdown
> interface g0/1
>  ipv6 address 2001:db8:acad:2::1/64
>  ipv6 address fe80::1 link-local
>  no shutdown
> end
> ```

> [!success]- ¿Por qué la puerta de enlace es link-local?
> El router anuncia el prefijo en su Router Advertisement usando su link-local como origen, y las PCs toman esa dirección como puerta de enlace. Las link-local siempre existen en el enlace, aunque cambie el prefijo global. Por eso conviene fijarla (`fe80::1`): así es la misma en todas las interfaces y fácil de reconocer.

## Notas personales

_Qué me costó, errores que cometí, dudas._
