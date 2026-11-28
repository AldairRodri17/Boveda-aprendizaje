---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Redes
herramienta: Cisco Packet Tracer
tags: [redes, laboratorio]
contenido: completo
---
# Lab 1 - Subredes IPv4 en Packet Tracer

Parte de: [[_Índice Direccionamiento IP]]

**Cubre:** [[IPv4 - estructura y clases]] · [[Máscara de subred y CIDR]] · [[Subnetting paso a paso]] · [[VLSM]] · [[Dirección de red y de broadcast]] · [[ARP]] · [[ICMP, ping y traceroute]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tu archivo `.pkt` en esta misma carpeta.

## Objetivo

Dividir `192.168.10.0/24` en subredes, configurar un router y dos LAN, y comprobar la comunicación entre redes.

## Topología

```
PC-A1 ─┐                     ┌─ PC-B1
       ├─ SW-A ── G0/0 R1 G0/1 ── SW-B ─┤
PC-A2 ─┘                     └─ PC-B2
```

Equipos: 1 router 2911, 2 switches 2960 y 4 PCs. Cables de cobre directos.

## Pasos

1. Calcula el plan: divide `192.168.10.0/24` en subredes **/26**. Usa la primera para la LAN A y la segunda para la LAN B. Anota la red, la puerta de enlace (primer host), el rango y el broadcast de cada una.
2. Arma la topología en Packet Tracer.
3. En R1 configura nombre, interfaces G0/0 y G0/1 con la primera dirección útil de cada subred, y enciéndelas.
4. Configura en cada PC (Desktop → IP Configuration) una IP estática, la máscara y la puerta de enlace de su LAN.
5. Verifica con los comandos de los criterios.
6. **Reto:** agrega una LAN C de 50 hosts en G0/2 usando la siguiente subred libre.

## Criterios de terminado

- [ ] Mi plan de direcciones coincide con la solución.
- [ ] `show ip interface brief` en R1 muestra G0/0 y G0/1 en **up/up** con sus IPs.
- [ ] PC-A1 hace `ping` a su puerta de enlace y a PC-B1 (responde).
- [ ] `tracert` de PC-A1 a PC-B1 muestra 2 saltos (R1 y el destino).
- [ ] `arp -a` en PC-A1 muestra la MAC de la puerta de enlace y `show ip arp` en R1 muestra las PCs.
- [ ] Puedo explicar por qué PC-A1 nunca pide por ARP la MAC de PC-B1.
- [ ] Reto: la LAN C funciona y hace ping a las otras dos.

## Solución

> [!success]- Plan de direcciones
> | LAN | Red | Puerta de enlace | Hosts | Broadcast |
> |---|---|---|---|---|
> | A | 192.168.10.0/26 | 192.168.10.1 | .1 – .62 | .63 |
> | B | 192.168.10.64/26 | 192.168.10.65 | .65 – .126 | .127 |
> | C (reto) | 192.168.10.128/26 | 192.168.10.129 | .129 – .190 | .191 |
>
> Ejemplo de PCs: A1 `.10`, A2 `.11`, B1 `.74`, B2 `.75`, todas con máscara `255.255.255.192`.

> [!success]- Configuración de R1
> ```
> enable
> configure terminal
> hostname R1
> interface g0/0
>  ip address 192.168.10.1 255.255.255.192
>  no shutdown
> interface g0/1
>  ip address 192.168.10.65 255.255.255.192
>  no shutdown
> end
> copy running-config startup-config
> ```

> [!success]- ¿Por qué PC-A1 no pide la MAC de PC-B1?
> Porque PC-B1 está en otra red: PC-A1 compara la IP destino con su máscara, ve que no es local y manda el paquete a su puerta de enlace. Solo necesita la MAC de R1 (G0/0).

## Notas personales

_Qué me costó, errores que cometí, dudas._
