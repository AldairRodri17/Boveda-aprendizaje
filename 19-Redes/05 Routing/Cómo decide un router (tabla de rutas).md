---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Cómo decide un router (tabla de rutas)

Parte de: [[19-Redes/05 Routing/_Índice Routing|Routing]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **tabla de rutas** (routing table o RIB) es la lista de redes que un router conoce y, para cada una, **por dónde enviar** los paquetes: el siguiente salto o la interfaz de salida. Cada vez que llega un paquete, el router consulta esta tabla con la IP destino y aplica reglas precisas para elegir una ruta. Entender esas reglas te permite predecir el camino de cualquier paquete leyendo `show ip route`.

### Cómo funciona
**Proceso de reenvío**:
1. Llega una trama dirigida a la MAC del router; se verifica el FCS y se extrae el paquete.
2. Se lee la **IP destino** y se busca en la tabla la **coincidencia de prefijo más larga** (*longest prefix match*): de todas las rutas que contienen al destino, gana la más específica (la de prefijo mayor).
3. Si no hay ninguna coincidencia y existe una **ruta por defecto** (`0.0.0.0/0`, la menos específica posible), se usa esa. Si no existe, se descarta el paquete y se envía ICMP *Destination Unreachable*.
4. Se decrementa el TTL, se recalcula el checksum, se resuelve con ARP la MAC del siguiente salto y se envía por la interfaz de salida en una trama nueva.

**Cómo se llena la tabla**: rutas **conectadas** (C) y **locales** (L, la /32 de la IP propia de cada interfaz) aparecen al configurar una IP en una interfaz `up/up`; **estáticas** (S) escritas a mano; y **dinámicas** aprendidas por protocolos (O para OSPF, D para EIGRP, B para BGP, R para RIP).

**Si dos fuentes ofrecen el mismo prefijo exacto**, primero gana la de menor **distancia administrativa** (AD): conectada 0, estática 1, EIGRP 90, OSPF 110, RIP 120 (ver [[Distancia administrativa y métrica]]). Si son del mismo protocolo, gana la de menor **métrica**. Si empatan en métrica, se instalan varias y se balancea (ECMP). Importante: la AD solo se compara entre rutas **al mismo prefijo**; un prefijo más largo siempre gana, aunque su AD sea mayor.

### Ejemplo
Tabla de R1:

```text
R1# show ip route
Codes: L - local, C - connected, S - static, O - OSPF, D - EIGRP, * - candidate default
Gateway of last resort is 203.0.113.1 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 203.0.113.1
O     172.16.0.0/16 [110/3] via 10.0.12.2, GigabitEthernet0/1
O     172.16.8.0/21 [110/2] via 10.0.13.2, GigabitEthernet0/2
S     172.16.9.0/24 [1/0] via 10.0.14.2
C     10.0.12.0/30 is directly connected, GigabitEthernet0/1
L     10.0.12.1/32 is directly connected, GigabitEthernet0/1
```

`[110/3]` es [AD/métrica]. ¿A dónde va cada destino?
- `172.16.9.40`: coincide con /16, /21 y /24 → gana **/24**, hacia `10.0.14.2`.
- `172.16.12.7`: está en `172.16.8.0/21` (8.0 a 15.255) y en /16 → gana **/21**, hacia `10.0.13.2`.
- `172.16.200.1`: solo coincide la /16 → hacia `10.0.12.2`.
- `192.168.50.1`: ninguna específica → **ruta por defecto** hacia `203.0.113.1`.

Verifícalo con Python:

```python
import ipaddress as ip
tabla = {"0.0.0.0/0": "203.0.113.1", "172.16.0.0/16": "10.0.12.2",
         "172.16.8.0/21": "10.0.13.2", "172.16.9.0/24": "10.0.14.2"}
def siguiente_salto(destino):
    d = ip.ip_address(destino)
    candidatas = [ip.ip_network(r) for r in tabla if d in ip.ip_network(r)]
    mejor = max(candidatas, key=lambda n: n.prefixlen)
    return mejor, tabla[str(mejor)]
for x in ["172.16.9.40", "172.16.12.7", "172.16.200.1", "192.168.50.1"]:
    print(x, siguiente_salto(x))
```

Y en el router: `show ip route 172.16.12.7` muestra exactamente qué entrada usará.

### Cuándo usarlo y cuándo no
Lee la tabla de rutas siempre que un destino no responda o tome un camino inesperado. Revisa también la del router **de regreso**. Para ver si una ruta está en la tabla pero no se usa por un prefijo más específico, compara con `show ip route longer-prefixes`. Recuerda que el reenvío real lo hace la FIB (CEF) derivada de esta tabla.

### Errores comunes
- **Elegir por AD antes que por prefijo**: primero gana el prefijo más largo.
- **Confundir "Gateway of last resort is not set"** con un error: solo significa que no hay ruta por defecto.
- **Olvidar las rutas locales (L)**: son /32 de la propia interfaz, no redes.
- **Pensar que una ruta conectada aparece con la interfaz caída**: solo si está `up/up`.

### Para profundizar
- [RFC 1812: requisitos para routers IPv4 (selección de rutas)](https://www.rfc-editor.org/rfc/rfc1812)
- [Cisco: Route Selection in Cisco Routers](https://www.cisco.com/c/en/us/support/docs/ip/enhanced-interior-gateway-routing-protocol-eigrp/8651-21.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Tabla: 10.0.0.0/8 → R2 · 10.1.0.0/16 → R3 · 10.1.1.0/24 → R4 · 0.0.0.0/0 → ISP. ¿A dónde va 10.1.1.77?
> **R4** (la /24 es la coincidencia más larga).

> [!question]- Misma tabla: ¿y 10.1.2.5?
> **R3** (coincide con /16 y /8; gana /16).

> [!question]- ¿Y 10.200.0.1?
> **R2** (solo coincide con 10.0.0.0/8).

> [!question]- ¿Y 172.16.0.1?
> **ISP** (solo coincide con la ruta por defecto).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué hace un router al recibir un paquete?
> Lee la IP destino, busca en la tabla de rutas la coincidencia **más específica** (prefijo más largo), reescribe la trama de capa 2 y lo envía por la interfaz de salida. Si no hay ruta ni default, lo descarta.

> [!question]- ¿Qué significan los códigos C, L, S, O y `*` en `show ip route`?
> **C** conectada · **L** local (la IP de la propia interfaz, /32) · **S** estática · **O** OSPF · `*` candidata a ruta por defecto.

> [!question]- Si hay dos rutas al mismo prefijo de distinto origen, ¿cuál gana?
> La de **menor distancia administrativa**. Si son del mismo protocolo, la de menor métrica.

## Práctica

- [[Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer]]

