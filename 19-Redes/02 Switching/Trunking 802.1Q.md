---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Trunking 802.1Q

Parte de: [[19-Redes/02 Switching/_Índice Switching|Switching]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **enlace troncal** (trunk) es un enlace punto a punto que transporta **varias VLAN** a la vez entre dos switches, o entre un switch y un router, un servidor de virtualización o un access point. Para saber a qué VLAN pertenece cada trama, se usa el estándar **IEEE 802.1Q**, que inserta una **etiqueta** en la trama Ethernet. (Cisco tenía un protocolo propietario, ISL, que ya está en desuso.)

### Cómo funciona
**La etiqueta 802.1Q** mide **4 bytes** y se inserta entre la MAC origen y el EtherType:

| Campo | Bits | Función |
|---|---|---|
| TPID | 16 | Valor `0x8100`: indica que la trama está etiquetada |
| PCP (prioridad) | 3 | Clase de servicio (CoS 0 a 7), usada por QoS |
| DEI | 1 | Elegible para descartar en congestión |
| VID | 12 | ID de VLAN (1 a 4094) |

Como la trama cambia, el switch **recalcula el FCS**. La trama máxima sube de 1518 a 1522 bytes.

**Reglas del trunk**:
- Todas las VLAN se etiquetan **excepto la nativa**, que viaja sin etiqueta (ver [[VLAN nativa]]).
- Por defecto un trunk permite **todas** las VLAN (1 a 4094). Es buena práctica limitarlo a las necesarias.
- **DTP** (Dynamic Trunking Protocol, de Cisco) puede negociar el trunk automáticamente. Modos: `dynamic auto`, `dynamic desirable`, `trunk`, `access`. Por seguridad se fija el modo y se desactiva DTP con `switchport nonegotiate`.
- En switches que soportan ISL y 802.1Q (como el 3560 o 3650 antiguos) hay que indicar `switchport trunk encapsulation dot1q` antes de `switchport mode trunk`. El 2960 solo soporta 802.1Q y no tiene ese comando.

### Ejemplo
Trunk entre SW1 y SW2 que solo permite las VLAN 10, 20 y 30, con nativa 999:

```text
SW1(config)# interface g0/1
SW1(config-if)# description Trunk a SW2
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 999
SW1(config-if)# switchport trunk allowed vlan 10,20,30
SW1(config-if)# switchport nonegotiate
SW1(config-if)# end
SW1# show interfaces trunk
Port   Mode  Encapsulation  Status    Native vlan
Gi0/1  on    802.1q         trunking  999
Port   Vlans allowed on trunk
Gi0/1  10,20,30
Port   Vlans allowed and active in management domain
Gi0/1  10,20,30
Port   Vlans in spanning tree forwarding state and not pruned
Gi0/1  10,20,30
```

Después llega la VLAN 40. **No** escribas `switchport trunk allowed vlan 40`: eso **reemplaza** la lista y deja solo la 40, cortando 10, 20 y 30. Lo correcto es:

```text
SW1(config-if)# switchport trunk allowed vlan add 40
```

También existen `remove`, `except`, `all` y `none`.

La última sección del `show interfaces trunk` es muy útil al diagnosticar: si una VLAN aparece en "allowed" pero no en "forwarding", puede que no exista en ese switch o que STP la esté bloqueando.

### Cuándo usarlo y cuándo no
Usa trunks entre switches, hacia el router en router on a stick, hacia APs con varios SSID y hacia hosts de virtualización (Hyper-V, VMware) que alojan VM en distintas VLAN. Para conectar PCs, impresoras o servidores de una sola VLAN usa puertos de acceso. Nunca dejes puertos de usuario en `dynamic desirable`: un atacante podría negociar un trunk (*switch spoofing*).

### Errores comunes
- **`allowed vlan` sin `add`**: borra las VLAN existentes del trunk.
- **Nativa distinta en cada lado**: fuga entre VLAN.
- **VLAN permitida pero no creada** en el switch remoto: no reenvía.
- **Dos extremos en `dynamic auto`**: ninguno inicia la negociación y el enlace queda en acceso.

### Para profundizar
- [IEEE 802.1 Working Group (802.1Q)](https://www.ieee802.org/1/)
- [Cisco Catalyst 2960: guías de configuración](https://www.cisco.com/c/en/us/support/switches/catalyst-2960-series-switches/products-installation-and-configuration-guides-list.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un enlace troncal?
> Un enlace que transporta varias VLANs a la vez, marcando cada trama con una etiqueta que indica su VLAN.

> [!question]- ¿Qué agrega 802.1Q a la trama?
> Una etiqueta de **4 bytes** entre la MAC origen y el EtherType: TPID `0x8100`, prioridad (PCP, 3 bits), DEI (1 bit) y **VLAN ID (12 bits)** → VLANs 1–4094.

> [!question]- ¿Qué comandos configuran un trunk que solo permita las VLANs 10, 20 y 99?
> ```
> interface g0/1
>  switchport mode trunk
>  switchport trunk native vlan 99
>  switchport trunk allowed vlan 10,20,99
> ```
> En switches que soportan ISL (p. ej. 3560) antes va `switchport trunk encapsulation dot1q`.

> [!question]- Error común: usar `switchport trunk allowed vlan 30` para agregar una VLAN. ¿Qué pasa?
> **Reemplaza** la lista y deja solo la 30. Para agregar se usa `switchport trunk allowed vlan add 30`.

> [!question]- ¿Con qué comando verificas los trunks?
> `show interfaces trunk` (modo, encapsulación, VLAN nativa y VLANs permitidas/activas).

## Práctica

- [[Lab 1 - VLANs, trunk y router on a stick en Packet Tracer]]

