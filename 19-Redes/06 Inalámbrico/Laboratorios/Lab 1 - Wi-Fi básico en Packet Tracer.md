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
# Lab 1 - Wi-Fi básico en Packet Tracer

Parte de: [[19-Redes/06 Inalámbrico/_Índice Inalámbrico|Inalámbrico]]

**Cubre:** [[Wi-Fi - estándares y bandas]] · [[WLAN - controladores y access points]] · [[Seguridad Wi-Fi (WPA2, WPA3 y 802.1X)]] · [[Access point]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Montar una WLAN segura con un router inalámbrico de Packet Tracer, conectar clientes y comprobar el acceso a la red cableada.

## Topología

```
Laptop1 )))         ┌── PC-CABLE
Tablet1 )))  WR1 ───┤ (LAN)
               │ Internet
              R1 (G0/0 192.168.50.1/24)
```

Equipos: 1 router inalámbrico (*Wireless Router* / WRT300N o Home Router, según tu versión), 1 router 2911, 1 laptop, 1 tablet y 1 PC.

## Pasos

1. Conecta el puerto **Internet** de WR1 a G0/0 de R1. Configura R1 con `192.168.50.1/24`.
2. En WR1 (pestaña GUI) → *Setup*: Internet con IP estática `192.168.50.2/24`, gateway `192.168.50.1`. Deja la LAN en `192.168.0.1/24` con DHCP activo.
3. En *Wireless*: cambia el SSID a `LAB-WIFI` y elige un canal que no se traslape (1, 6 u 11).
4. En *Wireless Security*: **WPA2 Personal** con cifrado **AES** y una clave de al menos 12 caracteres.
5. En la laptop: apágala, cambia el módulo de red por uno inalámbrico (p. ej. WPC300N), enciéndela y conéctate a `LAB-WIFI` desde *Desktop → PC Wireless*. Conecta también la tablet.
6. Cambia la contraseña de administración del router inalámbrico.
7. Verifica conectividad de los clientes inalámbricos con PC-CABLE y con R1.
8. **Reto (WLC):** si tu Packet Tracer incluye el controlador **WLC-2504** y APs ligeros (LAP), arma una segunda versión: WLC y LAP en un switch, crea la WLAN desde la GUI del WLC y conecta un cliente. (Pendiente de confirmar: los menús y modelos cambian entre versiones de Packet Tracer).

## Criterios de terminado

- [ ] La laptop y la tablet muestran conexión a `LAB-WIFI` con IP `192.168.0.x`.
- [ ] Con una contraseña incorrecta el cliente **no** se conecta.
- [ ] La laptop hace ping a PC-CABLE y a `192.168.50.1`.
- [ ] El SSID usa WPA2-AES (no WEP, no TKIP) y cambié la contraseña de administración.
- [ ] Puedo explicar por qué R1 ve el tráfico de los clientes con origen `192.168.50.2`.
- [ ] Reto: un cliente se conecta a una WLAN creada en el WLC (si mi versión lo permite).

## Solución

> [!success]- R1
> ```
> interface g0/0
>  ip address 192.168.50.1 255.255.255.0
>  no shutdown
> ```

> [!success]- Configuración del router inalámbrico (GUI)
> | Pestaña | Valor |
> |---|---|
> | Setup → Internet Setup | Static IP: 192.168.50.2 / 255.255.255.0, gateway 192.168.50.1 |
> | Setup → Network Setup | 192.168.0.1 / 255.255.255.0, DHCP Enabled |
> | Wireless → Basic | SSID `LAB-WIFI`, canal 1, 6 u 11 |
> | Wireless → Security | WPA2 Personal, AES, passphrase |
> | Administration | contraseña nueva |
> Pulsa **Save Settings** en cada pestaña.

> [!success]- ¿Por qué R1 ve 192.168.50.2?
> El router inalámbrico hace **NAT/PAT** de su LAN (`192.168.0.0/24`) hacia su puerto Internet. R1 solo ve la IP externa del router inalámbrico. Por eso tampoco necesita ruta a `192.168.0.0/24` para que respondan los pings.

> [!success]- Reto: idea general con WLC
> 1. WLC y LAP conectados a un switch en la VLAN de gestión; un DHCP (router o servidor) entrega IP a los LAP.
> 2. El LAP descubre al WLC y forma el túnel CAPWAP (el LED del AP cambia a verde cuando se une).
> 3. Desde un PC en la misma red, abre la GUI del WLC por navegador, crea la WLAN (SSID, WPA2-PSK) y asóciala a una interfaz o VLAN.
> 4. Conecta el cliente al nuevo SSID.
> Los nombres de los menús dependen de la versión de Packet Tracer.

## Notas personales

_Qué me costó, errores que cometí, dudas._
