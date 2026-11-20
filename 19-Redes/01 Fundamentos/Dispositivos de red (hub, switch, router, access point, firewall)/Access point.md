---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Access point

Parte de: [[Dispositivos de red (hub, switch, router, access point, firewall)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **access point** (AP o punto de acceso) es el dispositivo que permite a los equipos inalámbricos conectarse a una red cableada. Funciona como un **puente de capa 2** entre el medio de radio (802.11, Wi-Fi) y Ethernet: los clientes se asocian a un **SSID** y el AP entrega sus tramas a la LAN. No es un router: no enruta entre redes por sí mismo, aunque en casa suela venir dentro de la misma caja.

### Cómo funciona
1. El AP anuncia uno o varios SSID con tramas **beacon** periódicas en un canal de 2.4, 5 o 6 GHz.
2. El cliente (tu laptop o iPhone) descubre el SSID, se **autentica** (con WPA2/WPA3 Personal o Enterprise) y se **asocia** al AP.
3. Las tramas inalámbricas usan direcciones 802.11; el AP las convierte a tramas Ethernet y las envía por su puerto cableado al switch, y al revés.
4. Como el aire es un medio compartido, los clientes usan **CSMA/CA**: evitan colisiones en lugar de detectarlas.
5. Si el AP tiene varios SSID (por ejemplo, Corporativo e Invitados), cada uno se mapea a una VLAN y el puerto del switch debe ser **trunk**.

Hay dos modelos de AP:
- **Autónomo**: se configura uno por uno (SSID, seguridad, canales). Bien para 1 a 3 APs.
- **Ligero (lightweight)**: no guarda su configuración; la recibe de un **controlador inalámbrico (WLC)** mediante el protocolo **CAPWAP**. Ideal para decenas o cientos de APs con roaming, ajuste automático de canales y potencia, y políticas centralizadas. Se profundiza en [[WLAN - controladores y access points]].

Muchos APs se alimentan por **PoE** (Power over Ethernet) desde el switch, así no hace falta un contacto eléctrico en el techo.

### Ejemplo
En una oficina con un AP autónomo y dos SSID:

```text
SW1(config)# interface g0/10
SW1(config-if)# description AP-Sala-Juntas
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,30,99
SW1(config-if)# switchport trunk native vlan 99
```

- SSID `Itzamna-Corp` → VLAN 10 (WPA3 Enterprise).
- SSID `Itzamna-Invitados` → VLAN 30 (WPA3 Personal, solo internet).
- VLAN 99 para la gestión del AP.

Para tu dron DJI Mini 4K o la Osmo Pocket el principio es el mismo: el control remoto o la app crean un enlace inalámbrico punto a punto; un AP en cambio da acceso a toda una LAN.

En [[Cisco Packet Tracer]] usa el dispositivo AccessPoint-PT, configura el SSID y WPA2-PSK en su pestaña Config y conecta una laptop con tarjeta inalámbrica; el [[Lab 1 - Wi-Fi básico en Packet Tracer]] lo practica.

### Cuándo usarlo y cuándo no
Usa APs para dar movilidad y cobertura donde no conviene cablear. Un solo AP autónomo basta en casa o una oficina chica; con más de unos cuantos APs o necesidad de roaming fluido conviene un controlador o una solución administrada en la nube. Para equipos fijos con alto tráfico (servidores, PCs de edición de video), el cable sigue siendo mejor.

### Errores comunes
- **Conectar un AP con varios SSID a un puerto access**: solo funcionará una VLAN.
- **Poner muchos APs en el mismo canal**: interferencia entre ellos (cocanal).
- **Confundir AP con repetidor o "extensor"**: el extensor retransmite por radio y reduce el rendimiento a la mitad o menos.
- **Dejar la contraseña de administración por defecto** del AP.

### Para profundizar
- [IEEE 802.11 Working Group](https://www.ieee802.org/11/)
- [RFC 5415: CAPWAP](https://www.rfc-editor.org/rfc/rfc5415)
- [Wi-Fi Alliance](https://www.wi-fi.org/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace un access point?
> Conecta clientes inalámbricos a la red cableada: traduce entre tramas 802.11 y Ethernet.

> [!question]- ¿Qué diferencia hay entre un AP autónomo y uno ligero (lightweight)?
> El **autónomo** se configura uno por uno. El **ligero** depende de un controlador inalámbrico (WLC) que centraliza la configuración usando el protocolo CAPWAP.

## Práctica

- [[Lab 1 - Wi-Fi básico en Packet Tracer]]

