---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# VLAN nativa

Parte de: [[VLANs]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **VLAN nativa** es la VLAN cuyas tramas viajan **sin etiqueta 802.1Q** por un enlace troncal. Cuando un switch recibe una trama sin etiqueta en un trunk, la asigna a la VLAN nativa. Por defecto en Cisco es la **VLAN 1**. Existe por compatibilidad con equipos que no entienden etiquetas y para cierto tráfico de control. Es uno de los conceptos que más se pregunta en CCNA junto con [[Trunking 802.1Q]].

### Cómo funciona
- Al enviar por un trunk, el switch etiqueta todas las VLAN **excepto** la nativa.
- Al recibir una trama sin etiqueta por un trunk, la coloca en **su** VLAN nativa configurada.
- Por eso ambos extremos deben tener la **misma VLAN nativa**. Si no coinciden, ocurre una fuga: lo que el switch A manda sin etiqueta como "VLAN 1" el switch B lo recibe como "VLAN 99". El tráfico de una VLAN aparece en otra, se rompe la segmentación y STP puede comportarse de forma rara. Cisco detecta el problema con **CDP** y muestra mensajes como `%CDP-4-NATIVE_VLAN_MISMATCH`.

**Riesgo de seguridad: VLAN hopping por doble etiquetado**. Un atacante conectado a un puerto de acceso en la VLAN nativa envía una trama con dos etiquetas: la externa es la VLAN nativa y la interna la VLAN víctima. El primer switch quita la etiqueta externa (porque la nativa se envía sin etiqueta) y reenvía por el trunk la trama con la etiqueta interna; el segundo switch la entrega en la VLAN víctima. Es un ataque unidireccional, pero permite inyectar tráfico donde no debería.

Mitigaciones:
- Cambiar la nativa a una **VLAN sin uso** que no tenga puertos de acceso (por ejemplo, 999).
- No usar la VLAN 1 para usuarios ni gestión.
- Opcionalmente, etiquetar también la nativa con `vlan dot1q tag native` (si el equipo lo soporta).
- Desactivar la negociación DTP con `switchport nonegotiate` y poner los puertos de usuario en modo access.

### Ejemplo
Configurar la nativa 999 en ambos extremos del trunk entre SW1 y SW2:

```text
SW1(config)# vlan 999
SW1(config-vlan)# name NATIVA-SIN-USO
SW1(config-vlan)# exit
SW1(config)# interface g0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 999
SW1(config-if)# switchport nonegotiate
SW1(config-if)# end
SW1# show interfaces g0/1 trunk
Port        Mode         Encapsulation  Status        Native vlan
Gi0/1       on           802.1q         trunking      999
```

Repite exactamente lo mismo en SW2. Si solo lo haces en uno, verás el error de CDP en la consola de ambos.

En un router on a stick, la subinterfaz de la VLAN nativa se indica con la palabra `native`:

```text
R1(config)# interface g0/0.999
R1(config-subif)# encapsulation dot1Q 999 native
```

### Cuándo usarlo y cuándo no
Siempre que configures trunks, define la nativa explícitamente y de forma consistente en todo el dominio. Algunos access points o teléfonos esperan la gestión en la VLAN sin etiqueta: en ese caso, la nativa del puerto hacia el AP puede ser la VLAN de gestión de APs, pero documéntalo y que no sea la VLAN 1.

### Errores comunes
- **Nativa distinta en cada extremo**: fuga de tráfico entre VLAN y mensajes de CDP.
- **Usar como nativa una VLAN con usuarios**: facilita el VLAN hopping.
- **Pensar que la nativa no existe en el trunk**: sí viaja, solo que sin etiqueta.
- **Confundir "VLAN nativa" con "VLAN de administración"**: son conceptos distintos (aunque a veces coincidan).

### Para profundizar
- [IEEE 802.1 Working Group (802.1Q)](https://www.ieee802.org/1/)
- [Cisco Guide to Harden Cisco IOS Devices](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/13608-21.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es la VLAN nativa?
> La VLAN cuyas tramas viajan **sin etiqueta** por un enlace troncal 802.1Q. Por defecto es la VLAN 1.

> [!question]- ¿Qué pasa si la VLAN nativa no coincide en ambos extremos del trunk?
> El tráfico sin etiqueta termina en VLANs distintas en cada lado (fuga entre VLANs) y CDP muestra el aviso *native VLAN mismatch*.

> [!question]- ¿Por qué se recomienda cambiarla a una VLAN sin uso?
> Para mitigar el ataque de *double tagging* (VLAN hopping) y no mezclar tráfico de usuarios sin etiquetar: `switchport trunk native vlan 99`.

## Práctica

- [[Lab 1 - VLANs, trunk y router on a stick en Packet Tracer]]

