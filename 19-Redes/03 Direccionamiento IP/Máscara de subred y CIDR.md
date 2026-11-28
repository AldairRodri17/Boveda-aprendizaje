---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# Máscara de subred y CIDR

Parte de: [[_Índice Direccionamiento IP]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **máscara de subred** es un valor de 32 bits que indica qué parte de una dirección IPv4 es **red** (bits en 1) y qué parte es **host** (bits en 0). Los unos siempre van juntos a la izquierda. **CIDR** (Classless Inter-Domain Routing) introdujo la notación de **prefijo** `/n`, donde n es el número de bits en 1: `/24` equivale a `255.255.255.0`. Con la máscara un equipo sabe si un destino está en su propia red o si debe mandarlo al gateway.

### Cómo funciona
**Valores posibles de un octeto de máscara**: 0, 128, 192, 224, 240, 248, 252, 254, 255. Cada uno agrega un bit en 1.

| Prefijo | Máscara | Hosts utilizables | Tamaño de bloque en el octeto interesante |
|---|---|---|---|
| /24 | 255.255.255.0 | 254 | 256 (cuarto octeto) |
| /25 | 255.255.255.128 | 126 | 128 |
| /26 | 255.255.255.192 | 62 | 64 |
| /27 | 255.255.255.224 | 30 | 32 |
| /28 | 255.255.255.240 | 14 | 16 |
| /29 | 255.255.255.248 | 6 | 8 |
| /30 | 255.255.255.252 | 2 | 4 |
| /21 | 255.255.248.0 | 2046 | 8 (tercer octeto) |
| /22 | 255.255.252.0 | 1022 | 4 (tercer octeto) |

**Hosts por subred** = 2^(bits de host) − 2. Se restan 2 porque la primera dirección (todos los bits de host en 0) es la **dirección de red** y la última (todos en 1) es la de **broadcast**; ninguna se asigna a un equipo (ver [[Dirección de red y de broadcast]]). Excepciones: en enlaces punto a punto se puede usar /31 (RFC 3021, 2 direcciones sin red ni broadcast) y /32 identifica un solo host (loopbacks, rutas de host).

**Cómo usa la máscara un host**: hace un AND binario entre su IP y su máscara para obtener su red, y lo mismo con la IP destino. Si los resultados coinciden, el destino es local (ARP directo); si no, envía al gateway.

**Wildcard**: es la máscara invertida (255 menos cada octeto). Para /24 es `0.0.0.255`; para /27, `0.0.0.31`. La usan las **ACL** y el comando `network` de **OSPF** en Cisco IOS: un 0 significa "este bit debe coincidir" y un 1 "no importa".

**CIDR también agrega**: permite resumir varias redes en una ruta (supernetting). Por ejemplo, `192.168.0.0/24` a `192.168.3.0/24` se resumen como `192.168.0.0/22`.

### Ejemplo
Equipo con `10.20.30.77/27`:
- Máscara: `255.255.255.224`; bloque de 32 en el cuarto octeto.
- 77 cae entre 64 y 95 → red `10.20.30.64`, broadcast `10.20.30.95`, hosts `.65` a `.94` (30 hosts).
- Wildcard: `0.0.0.31`.

Verificado con Python:

```python
import ipaddress
n = ipaddress.ip_interface("10.20.30.77/27").network
print(n, n.netmask, n.hostmask, n.num_addresses - 2)
# 10.20.30.64/27 255.255.255.224 0.0.0.31 30
```

En Cisco IOS la máscara se escribe en decimal al configurar la interfaz y la wildcard en ACL y OSPF:

```text
R1(config-if)# ip address 10.20.30.65 255.255.255.224
R1(config)# access-list 10 permit 10.20.30.64 0.0.0.31
R1(config-router)# network 10.20.30.64 0.0.0.31 area 0
```

En Azure o en Docker escribirás directamente la notación CIDR (`10.20.30.64/27`).

### Cuándo usarlo y cuándo no
Siempre que configures una IP necesitas su máscara. Elige el prefijo según cuántos hosts necesitas con margen de crecimiento: /24 para VLAN de usuarios típicas, /30 o /31 para enlaces entre routers, /32 para loopbacks. Para dividir un bloque en subredes de distinto tamaño usa [[VLSM]].

### Errores comunes
- **Olvidar restar 2** al calcular hosts.
- **Escribir una máscara con unos no contiguos** (como 255.255.0.255): no es válida.
- **Usar la máscara donde va la wildcard** en una ACL: `access-list 10 permit 10.0.0.0 255.0.0.0` hace lo contrario de lo esperado.
- **Máscaras distintas en hosts de la misma red**: unos se creen locales y otros no.

### Para profundizar
- [RFC 4632: CIDR](https://www.rfc-editor.org/rfc/rfc4632)
- [RFC 950: procedimiento de subnetting](https://www.rfc-editor.org/rfc/rfc950)
- [Documentación de Python: módulo ipaddress](https://docs.python.org/es/3/library/ipaddress.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué indica una máscara de subred?
> Qué bits de la dirección son de **red** (1) y cuáles son de **host** (0).

> [!question]- Escribe /26 en decimal y di cuántos hosts admite.
> `255.255.255.192` · 6 bits de host → 2⁶ − 2 = **62 hosts**.

> [!question]- ¿Por qué se restan 2 al calcular hosts?
> Porque la primera dirección es la de **red** y la última la de **broadcast**; no se asignan a equipos. (Excepciones: /31 en enlaces punto a punto y /32 para un solo host.)

> [!question]- ¿Qué es la wildcard de una /24 y dónde se usa?
> `0.0.0.255`: la máscara invertida. Se usa en ACLs y en la configuración de OSPF en Cisco.

## Práctica

- [[Lab 1 - Subredes IPv4 en Packet Tracer]]

