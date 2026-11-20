---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Loopback y APIPA

Parte de: [[Direcciones públicas y privadas]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son dos bloques IPv4 de uso especial que no sirven para comunicarse con otros equipos de la forma normal:
- **Loopback** (`127.0.0.0/8`, sobre todo `127.0.0.1`): la dirección del **propio equipo**. Lo que se envía ahí nunca sale por la tarjeta de red; se procesa dentro de la pila TCP/IP.
- **APIPA** o link-local (`169.254.0.0/16`): la dirección que un equipo **se autoasigna** cuando está configurado para DHCP y no obtiene respuesta de ningún servidor.

### Cómo funciona
**Loopback**:
- Todo el bloque `127.0.0.0/8` está reservado, aunque casi siempre se usa `127.0.0.1`; el nombre `localhost` resuelve ahí (y a `::1` en IPv6).
- Hacer `ping 127.0.0.1` comprueba que la pila TCP/IP del sistema operativo funciona, sin probar el cable ni la red.
- En desarrollo lo usas a diario: `https://localhost:5001` para tu API de ASP.NET Core o `http://localhost:5173` para el servidor de Vite de Itzamna.Clio.
- No confundir con la **interfaz loopback de un router Cisco** (`interface loopback 0`): es una interfaz virtual que siempre está activa y que sí tiene una IP enrutable (por ejemplo `1.1.1.1/32` o `10.255.0.1/32`); se usa como router ID de OSPF, para gestión y para BGP.

**APIPA (169.254.0.0/16)**:
1. El equipo envía DHCP Discover y no recibe Offer.
2. Tras reintentar, elige al azar una dirección `169.254.x.y` (excluyendo la primera y la última /24 del bloque), comprueba con ARP que nadie la usa y se la asigna con máscara `255.255.0.0`.
3. Sigue intentando DHCP en segundo plano.

Un equipo con APIPA puede hablar con otros equipos con APIPA **en el mismo segmento**, pero **no tiene gateway**, así que no sale a internet ni a otras subredes. Los routers no reenvían este rango.

### Ejemplo
Un usuario dice "no tengo internet". En su PC:

```text
C:\> ipconfig
   Dirección IPv4 de configuración automática: 169.254.23.5
   Máscara de subred: 255.255.0.0
   Puerta de enlace predeterminada:
```

Eso te dice: **el equipo no recibió respuesta del DHCP**. Posibles causas, de abajo hacia arriba: cable o puerto caído (capa 1), puerto en la VLAN equivocada o bloqueado por port security (capa 2), servidor DHCP caído o pool agotado, falta de DHCP relay en el router (`ip helper-address`) o DHCP snooping bloqueando las respuestas. Prueba:

```text
C:\> ping 127.0.0.1          # la pila TCP/IP funciona
C:\> ipconfig /release
C:\> ipconfig /renew         # vuelve a pedir dirección
```

Y en el router o switch:

```text
R1# show ip dhcp pool
R1# show ip dhcp binding
R1# show ip interface g0/0.10 | include Helper
```

### Cuándo usarlo y cuándo no
- Loopback (127.x): pruebas locales, servicios que solo deben escuchar en el propio equipo (por ejemplo, una base de datos de desarrollo enlazada a `127.0.0.1` para que no sea accesible desde la red).
- Interfaz loopback en routers: identificadores estables (router ID, gestión, BGP).
- APIPA: nunca como configuración deseada en una red empresarial; es un **síntoma** de que DHCP falló. En redes muy simples sin servidor (dos equipos conectados directamente) puede permitir comunicación básica.

### Errores comunes
- **Pensar que `ping 127.0.0.1` prueba la red**: solo prueba la pila local.
- **Ignorar una 169.254**: es una pista directa hacia DHCP.
- **Publicar un servicio en `127.0.0.1` y esperar acceso desde otro equipo**: escucha en `0.0.0.0` o en la IP de la interfaz.
- **Confundir APIPA (169.254.x.x) con IPv6 link-local** (fe80::): misma idea, distinto protocolo.

### Para profundizar
- [RFC 3927: configuración dinámica de direcciones IPv4 link-local](https://www.rfc-editor.org/rfc/rfc3927)
- [RFC 1122: requisitos de hosts (loopback)](https://www.rfc-editor.org/rfc/rfc1122)
- [RFC 6890: direcciones de propósito especial](https://www.rfc-editor.org/rfc/rfc6890)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es 127.0.0.1?
> La dirección de **loopback**: el propio equipo. Todo `127.0.0.0/8` está reservado. En IPv6 es `::1`.

> [!question]- Un equipo tiene 169.254.23.5. ¿Qué te dice?
> Es **APIPA** (`169.254.0.0/16`): el equipo no consiguió dirección de un servidor DHCP y se autoasignó una. Revisa DHCP o el cable.

> [!question]- ¿Un equipo con APIPA puede salir a internet?
> No: no tiene puerta de enlace y ese rango no se enruta.

