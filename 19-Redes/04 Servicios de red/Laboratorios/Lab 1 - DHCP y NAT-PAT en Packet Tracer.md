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
# Lab 1 - DHCP y NAT-PAT en Packet Tracer

Parte de: [[19-Redes/04 Servicios de red/_Índice Servicios de red|Servicios de red]]

**Cubre:** [[DHCP]] · [[Proceso DORA de DHCP]] · [[DHCP relay]] · [[NAT y PAT]] · [[NAT estático]] · [[PAT (sobrecarga de NAT)]] · [[DNS en la red (resolución, registros y servidores)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Hacer que R1 entregue direcciones por DHCP a la LAN y que la LAN salga a "Internet" con PAT; publicar un servidor interno con NAT estático.

## Topología

```
PC1 ─┐
PC2 ─┼─ SW1 ── G0/0 R1 G0/1 ─────── G0/0 ISP G0/1 ── SW2 ── WEB-EXT
SRV ─┘   192.168.1.0/24   203.0.113.0/29       198.51.100.0/24
```

| Equipo | Interfaz | IP |
|---|---|---|
| R1 | G0/0 | 192.168.1.1/24 |
| R1 | G0/1 | 203.0.113.2/29 |
| ISP | G0/0 | 203.0.113.1/29 |
| ISP | G0/1 | 198.51.100.1/24 |
| WEB-EXT (Server-PT) | Fa0 | 198.51.100.10/24, gateway .1, servicios HTTP y DNS |
| SRV (Server-PT interno) | Fa0 | 192.168.1.5/24, gateway .1, HTTP |

## Pasos

1. Arma la topología con 2 routers 2911, 2 switches, 2 PCs y 2 servidores. Configura las IPs de la tabla.
2. En WEB-EXT activa DNS y crea un registro A: `www.externo.com` → `198.51.100.10`.
3. En R1 crea un pool DHCP para la LAN: excluye `.1` a `.10`, puerta de enlace `.1` y DNS `198.51.100.10`.
4. En las PCs elige **DHCP** y verifica la IP recibida. En modo simulación observa los cuatro mensajes DORA.
5. En R1 crea una ruta por defecto hacia el ISP. El ISP **no** debe tener rutas hacia `192.168.1.0/24`.
6. Configura PAT usando la IP de G0/1 para toda la LAN.
7. Publica SRV como `203.0.113.3` con NAT estático.
8. Prueba desde PC1 el navegador a `http://www.externo.com` y desde WEB-EXT a `http://203.0.113.3`.
9. **Reto (relay):** mueve el DHCP a SRV (servicio DHCP de Server-PT) y usa `ip helper-address` en R1 como si SRV estuviera en otra red; documenta qué cambiaría.

## Criterios de terminado

- [ ] PC1 y PC2 reciben IP entre `192.168.1.11` y `.254`, gateway `.1` y DNS `198.51.100.10`.
- [ ] `show ip dhcp binding` en R1 lista las dos PCs.
- [ ] Vi en simulación Discover, Offer, Request y Ack.
- [ ] PC1 abre `http://www.externo.com`.
- [ ] `show ip nat translations` muestra entradas de PAT con `203.0.113.2:<puerto>` y la estática `203.0.113.3 ↔ 192.168.1.5`.
- [ ] WEB-EXT abre `http://203.0.113.3` (llega a SRV).
- [ ] Puedo explicar por qué el ISP no necesita ruta a la red privada.

## Solución

> [!success]- DHCP en R1
> ```
> ip dhcp excluded-address 192.168.1.1 192.168.1.10
> ip dhcp pool LAN
>  network 192.168.1.0 255.255.255.0
>  default-router 192.168.1.1
>  dns-server 198.51.100.10
> ```

> [!success]- Ruta por defecto, PAT y NAT estático en R1
> ```
> ip route 0.0.0.0 0.0.0.0 203.0.113.1
> access-list 1 permit 192.168.1.0 0.0.0.255
> ip nat inside source list 1 interface g0/1 overload
> ip nat inside source static 192.168.1.5 203.0.113.3
> interface g0/0
>  ip nat inside
> interface g0/1
>  ip nat outside
> ```
> El ISP solo ve direcciones `203.0.113.x`, que están en su red conectada; por eso no necesita ruta a `192.168.1.0/24`.

> [!success]- Reto: DHCP relay
> En un diseño real SRV estaría en otra subred (p. ej. `192.168.2.0/24` en G0/2). R1 llevaría en la interfaz de los clientes:
> ```
> interface g0/0
>  ip helper-address 192.168.2.5
> ```
> y en el servidor un pool con red `192.168.1.0`, gateway `192.168.1.1`. R1 pone `192.168.1.1` como *giaddr* y el servidor elige el pool por esa red.

> [!success]- Si algo no funciona
> - PCs con `169.254.x.x`: el pool está mal o G0/0 está apagada.
> - DHCP sí, Internet no: falta `ip nat inside/outside`, la ACL no coincide o falta la ruta por defecto.
> - Nombres no resuelven pero la IP sí: revisa el registro A en WEB-EXT y el `dns-server` del pool.

## Notas personales

_Qué me costó, errores que cometí, dudas._
