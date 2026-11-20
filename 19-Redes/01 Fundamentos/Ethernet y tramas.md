---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Ethernet y tramas

Parte de: [[19-Redes/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Ethernet** es la familia de tecnologías de capa 1 y 2 definida por el estándar IEEE 802.3 y es, por mucho, la más usada en redes locales cableadas. Define los medios físicos (cobre y fibra), las velocidades (de 10 Mbps a 400 Gbps y más) y el formato de la **trama**, que es la unidad de datos de capa 2 que transporta paquetes IP, ARP y otros protocolos.

### Cómo funciona
Formato de una trama **Ethernet II** (la que lleva IP en casi todas las redes):

| Campo | Tamaño | Para qué sirve |
|---|---|---|
| Preámbulo + SFD | 7 + 1 bytes | Sincronizar relojes y marcar el inicio (no se cuenta en el tamaño de trama) |
| MAC destino | 6 bytes | A quién va la trama |
| MAC origen | 6 bytes | Quién la envía |
| EtherType | 2 bytes | Qué protocolo va dentro |
| Datos (payload) | 46 a 1500 bytes | El paquete de capa 3 |
| FCS | 4 bytes | CRC para detectar errores |

**Tamaños**: mínimo **64 bytes** y máximo **1518 bytes** (de MAC destino a FCS). Si el paquete es menor de 46 bytes, se rellena (padding). Con etiqueta 802.1Q (4 bytes extra) el máximo sube a 1522. Las **jumbo frames** (hasta unos 9000 bytes) son una extensión no estándar que debe habilitarse en todo el camino.

**EtherType**: le dice al receptor a qué protocolo de capa 3 entregar los datos. Valores comunes: `0x0800` IPv4, `0x86DD` IPv6, `0x0806` ARP, `0x8100` etiqueta 802.1Q. Si el valor es 1500 o menor, el campo se interpreta como longitud (formato 802.3 con LLC), usado por protocolos antiguos y por STP.

**FCS**: el emisor calcula un CRC-32 sobre la trama; el receptor lo recalcula. Si no coincide, el switch o la tarjeta **descartan la trama en silencio** e incrementan el contador de errores CRC; no avisan al emisor. La recuperación, si existe, la hace TCP más arriba.

Una trama más chica de 64 bytes es un **runt** (normalmente fruto de colisiones) y una más grande que el máximo permitido es un **giant**.

### Ejemplo
Abre [[Wireshark (captura y análisis de tráfico)]], captura en tu tarjeta y haz `ping 192.168.1.1`. Selecciona un paquete ICMP:

```text
Ethernet II, Src: Intel_12:34:56 (3c:52:82:12:34:56), Dst: Cisco_a2:0b:01 (00:19:e8:a2:0b:01)
    Destination: 00:19:e8:a2:0b:01
    Source: 3c:52:82:12:34:56
    Type: IPv4 (0x0800)
Internet Protocol Version 4, Src: 192.168.1.20, Dst: 192.168.1.1
```

Wireshark normalmente no muestra el FCS porque la tarjeta lo quita antes de entregarlo al sistema operativo. En un switch Cisco, los errores de FCS aparecen como `CRC` en `show interfaces`:

```text
SW1# show interfaces g0/1 | include CRC|runts
     0 runts, 0 giants, 0 throttles
     37 input errors, 37 CRC, 0 frame, 0 overrun, 0 ignored
```

### Cuándo usarlo y cuándo no
Ethernet es la opción por defecto para cualquier red cableada: oficinas, centros de datos e incluso redes industriales y automotrices. Conocer la trama te sirve para entender VLAN (802.1Q inserta su etiqueta después de la MAC origen), MTU en túneles y para leer capturas. Para enlaces WAN de proveedor se usan también Ethernet metropolitano o tecnologías como MPLS que transportan tramas Ethernet.

### Errores comunes
- **Confundir el MTU de IP (1500) con el tamaño de trama (1518)**: el MTU se refiere al payload.
- **Activar jumbo frames solo en un equipo**: los equipos intermedios descartan las tramas grandes.
- **Interpretar los errores CRC como problema de configuración**: casi siempre son de capa 1 (cable, conector, dúplex).
- **Olvidar el padding**: una trama con un ping pequeño sigue midiendo al menos 64 bytes.

### Para profundizar
- [IEEE 802.3 Ethernet Working Group](https://www.ieee802.org/3/)
- [IANA: registro de EtherTypes (IEEE 802 Numbers)](https://www.iana.org/assignments/ieee-802-numbers/ieee-802-numbers.xhtml)
- [RFC 894: IP sobre Ethernet](https://www.rfc-editor.org/rfc/rfc894)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué campos tiene una trama Ethernet II?
> Preámbulo (7 B) + SFD (1 B) · MAC destino (6 B) · MAC origen (6 B) · EtherType (2 B) · Datos (46–1500 B) · FCS (4 B).

> [!question]- ¿Cuál es el tamaño mínimo y máximo de una trama Ethernet?
> Mínimo **64 bytes** y máximo **1518 bytes** (1522 con etiqueta 802.1Q), sin contar preámbulo. Una trama de menos de 64 B es un *runt*.

> [!question]- ¿Para qué sirve el EtherType?
> Indica qué protocolo viene dentro: `0x0800` IPv4, `0x86DD` IPv6, `0x0806` ARP, `0x8100` etiqueta 802.1Q.

> [!question]- ¿Qué hace un switch si el FCS de una trama no coincide?
> La descarta (la trama llegó corrupta). Se ve como errores de CRC en `show interfaces`.

## Práctica

- [[Lab 1 - Encapsulamiento, hub y switch en modo simulación]]
- [[Lab 2 - Wireshark capturando ARP, ICMP y DNS]]

