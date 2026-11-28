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
# Dirección de red y de broadcast

Parte de: [[Direcciones públicas y privadas]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
En cada subred IPv4 hay dos direcciones que **no se asignan a ningún equipo**:
- La **dirección de red**: la primera del bloque, con todos los bits de host en 0. Identifica a la subred en las tablas de rutas.
- La **dirección de broadcast** (dirigido): la última del bloque, con todos los bits de host en 1. Un paquete enviado a ella llega a todos los hosts de esa subred.

Por eso el número de hosts utilizables es 2ʰ − 2.

### Cómo funciona
**En binario**, con `172.16.40.130/26`:
- Máscara /26 → 26 bits de red y 6 de host.
- IP en binario (cuarto octeto): `130 = 10000010`. Los dos primeros bits (`10`) son de red; los seis últimos (`000010`), de host.
- Red: bits de host en 0 → `10000000` = **128** → `172.16.40.128`.
- Broadcast: bits de host en 1 → `10111111` = **191** → `172.16.40.191`.
- Hosts: `172.16.40.129` a `172.16.40.190` (62 hosts).

**Atajo con el número mágico**: /26 → 256 − 192 = 64; los bloques empiezan en 0, 64, 128, 192. 130 cae en el bloque de 128; el siguiente empieza en 192, así que el broadcast es 191.

**Tipos de broadcast IPv4**:
- **Limitado** `255.255.255.255`: "todos en este segmento". Nunca cruza un router. Lo usa un cliente DHCP que aún no tiene IP.
- **Dirigido** (de subred), por ejemplo `172.16.40.191`: dirigido a todos los hosts de una subred concreta. Por defecto, los routers Cisco modernos **no** lo reenvían hacia la subred destino (`no ip directed-broadcast` es el valor predeterminado desde IOS 12.0), porque se usaba en ataques de amplificación como **Smurf**. Se puede habilitar en casos muy específicos (por ejemplo, Wake-on-LAN entre subredes), con cuidado.

### Ejemplo
Ejercicio rápido: red y broadcast de `10.0.7.200/22`.
- /22 → máscara `255.255.252.0`; octeto interesante: el tercero; número mágico 4.
- Múltiplos de 4: 0, 4, 8… 7 cae en el bloque de **4**.
- Red **10.0.4.0**, broadcast **10.0.7.255**, hosts `10.0.4.1` a `10.0.7.254` (1,022 hosts).
- Fíjate: aquí `10.0.5.0` y `10.0.6.255` **sí** son hosts válidos, aunque terminen en 0 o 255.

```python
import ipaddress
for s in ["172.16.40.130/26", "10.0.7.200/22"]:
    n = ipaddress.ip_interface(s).network
    print(s, "red", n.network_address, "broadcast", n.broadcast_address)
```

En la tabla de rutas de un router verás siempre la **dirección de red** con su prefijo:

```text
R1# show ip route connected
C    172.16.40.128/26 is directly connected, GigabitEthernet0/1
L    172.16.40.129/32 is directly connected, GigabitEthernet0/1
```

La `L` es la ruta local de la IP propia de la interfaz (/32).

### Cuándo usarlo y cuándo no
Calcula red y broadcast siempre que asignes IPs, definas un pool de DHCP, escribas una ACL o un `network` de OSPF (que usan la dirección de red con wildcard). Nunca asignes la red o el broadcast a un equipo; IOS te lo impedirá en las interfaces, pero un servidor Windows o Linux puede aceptarlo y comportarse de forma errática.

### Errores comunes
- **Creer que toda dirección terminada en .0 o .255 es inválida**: depende del prefijo.
- **Confundir broadcast limitado y dirigido**.
- **Habilitar `ip directed-broadcast` sin necesidad**: expone a ataques de amplificación.
- **Usar la IP de host en lugar de la de red** en `network` de OSPF o en una ACL sin la wildcard correcta.

### Para profundizar
- [RFC 919: broadcast en datagramas de internet](https://www.rfc-editor.org/rfc/rfc919)
- [RFC 2644: cambio del valor por defecto de directed broadcast en routers](https://www.rfc-editor.org/rfc/rfc2644)
- [RFC 950: subnetting](https://www.rfc-editor.org/rfc/rfc950)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se ven en binario la dirección de red y la de broadcast?
> Red: todos los bits de host en **0**. Broadcast: todos en **1**.

> [!question]- Red y broadcast de 192.168.10.64/26.
> Red `192.168.10.64` · Broadcast `192.168.10.127`.

> [!question]- ¿Un router reenvía un broadcast a otra red?
> No. El broadcast limitado `255.255.255.255` nunca cruza routers, y el dirigido está deshabilitado por omisión en Cisco (`no ip directed-broadcast`).

## Práctica

- [[Lab 1 - Subredes IPv4 en Packet Tracer]]

