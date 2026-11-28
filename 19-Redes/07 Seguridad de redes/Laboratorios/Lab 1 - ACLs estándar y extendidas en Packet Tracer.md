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
# Lab 1 - ACLs estándar y extendidas en Packet Tracer

Parte de: [[19-Redes/07 Seguridad de redes/_Índice Seguridad de redes|Seguridad de redes]]

**Cubre:** [[ACLs (estándar y extendidas)]] · [[ACL estándar]] · [[ACL extendida]] · [[ACL nombrada]] · [[Segmentación y DMZ]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Aplicar una ACL estándar y una extendida nombrada para cumplir una política de acceso entre tres redes, y verificarla con contadores.

## Topología

```
VENTAS 192.168.10.0/24 ── G0/0
                              R1 ── G0/2 ── SERVIDORES 192.168.30.0/24 (SRV1 .10)
TI     192.168.20.0/24 ── G0/1
```

PCs: PC-V1 `192.168.10.10`, PC-V2 `192.168.10.50`, PC-T1 `192.168.20.10`. SRV1 (Server-PT) con HTTP y FTP activos. Puertas de enlace: `.1` en cada red.

## Política

1. Solo la red **TI** puede abrir sesiones remotas (Telnet/SSH) a R1.
2. **VENTAS** puede usar solo **HTTP** y **ping** hacia SRV1; nada más hacia la red de servidores. Lo demás (p. ej. hacia TI) está permitido.
3. El equipo **PC-V2** no debe llegar a la red de servidores de ninguna forma.
4. TI tiene acceso total.

## Pasos

1. Arma la topología y configura las IPs. Comprueba que todo hace ping con todo antes de crear ACLs.
2. Configura en R1 `enable secret`, usuario local y líneas VTY con `login local` (`transport input telnet ssh`).
3. Escribe la ACL estándar para el punto 1 y aplícala a las líneas VTY.
4. Escribe una ACL extendida **nombrada** `VENTAS-IN` para el punto 2 y aplícala donde corresponde.
5. Escribe una ACL estándar para el punto 3 y aplícala donde corresponde.
6. Prueba cada regla y revisa los contadores.
7. **Reto:** permite también HTTPS a SRV1 insertando una línea en `VENTAS-IN` sin reescribir la ACL.

## Criterios de terminado

- [ ] Desde PC-T1, `telnet 192.168.20.1` pide usuario; desde PC-V1 la conexión es rechazada.
- [ ] PC-V1 abre `http://192.168.30.10` y hace ping a SRV1.
- [ ] PC-V1 **no** puede usar FTP a SRV1 (`ftp 192.168.30.10` falla).
- [ ] PC-V1 hace ping a PC-T1.
- [ ] PC-V2 no llega a SRV1 ni por ping ni por HTTP.
- [ ] `show access-lists` muestra coincidencias (*matches*) en las líneas esperadas.
- [ ] Puedo explicar por qué cada ACL quedó en esa interfaz y dirección.

## Solución

> [!success]- Punto 1: VTY solo desde TI (estándar)
> ```
> username admin secret Adm1n!
> access-list 10 permit 192.168.20.0 0.0.0.255
> line vty 0 4
>  login local
>  transport input telnet ssh
>  access-class 10 in
> ```

> [!success]- Punto 2: VENTAS-IN (extendida nombrada, cerca del origen)
> ```
> ip access-list extended VENTAS-IN
>  10 permit tcp 192.168.10.0 0.0.0.255 host 192.168.30.10 eq 80
>  20 permit icmp 192.168.10.0 0.0.0.255 host 192.168.30.10 echo
>  30 deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
>  40 permit ip any any
> interface g0/0
>  ip access-group VENTAS-IN in
> ```
> Las respuestas de SRV1 (echo-reply, tráfico web de regreso) no pasan por esta ACL porque entran a R1 por G0/2.

> [!success]- Punto 3: bloquear PC-V2 (estándar, cerca del destino)
> ```
> access-list 1 deny host 192.168.10.50
> access-list 1 permit any
> interface g0/2
>  ip access-group 1 out
> ```
> Al estar en la salida hacia servidores, PC-V2 sigue pudiendo llegar a TI.

> [!success]- Reto: insertar HTTPS
> ```
> ip access-list extended VENTAS-IN
>  15 permit tcp 192.168.10.0 0.0.0.255 host 192.168.30.10 eq 443
> ```
> Verifica el orden con `show access-lists VENTAS-IN`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
