---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
contenido: completo
---
# Rutas estáticas y ruta por defecto

Parte de: [[19-Redes/05 Routing/_Índice Routing|Routing]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **ruta estática** es una ruta que el administrador escribe a mano en el router: "para llegar a esta red, envía al siguiente salto X" (o por la interfaz Y). Una **ruta por defecto** es una estática especial, `0.0.0.0/0`, que se usa cuando ninguna otra coincide; normalmente apunta hacia internet o hacia el núcleo. Son simples, predecibles y no consumen CPU ni ancho de banda, pero no se adaptan solas a los cambios.

### Cómo funciona
Sintaxis en Cisco IOS:

```text
ip route <red> <máscara> <siguiente salto | interfaz de salida> [distancia administrativa]
ipv6 route <prefijo/longitud> <siguiente salto | interfaz> [distancia]
```

Tipos:
- **Con siguiente salto** (`ip route 10.2.0.0 255.255.0.0 10.0.12.2`): la más común. El router busca cómo llegar al siguiente salto (búsqueda recursiva) y resuelve su MAC.
- **Con interfaz de salida** (`... g0/1`): útil en enlaces punto a punto seriales. En Ethernet obliga a hacer ARP por cada destino (proxy ARP), por eso se evita.
- **Totalmente especificada** (interfaz + siguiente salto): recomendada en Ethernet y en IPv6 con siguiente salto link-local.
- **Ruta por defecto**: `ip route 0.0.0.0 0.0.0.0 <salto>`; en la tabla aparece como `S*` y como *Gateway of last resort*.
- **Ruta estática flotante**: una estática con **AD mayor** que la ruta principal (por ejemplo, 200 frente a OSPF 110 o frente a otra estática de AD 1). No se instala mientras exista la principal; aparece solo si esta desaparece. Sirve de respaldo.
- **Ruta de host**: /32, hacia un solo equipo.

**La ruta de regreso**: el enrutamiento es **unidireccional**. Si R1 sabe llegar a la LAN de R3 pero R3 no sabe volver a la LAN de R1, el ping sale y la respuesta se pierde. Siempre configura (o anuncia) ambos sentidos.

Una estática con siguiente salto solo se instala si el router tiene ruta hacia ese siguiente salto; si la interfaz cae, la estática desaparece de la tabla (y entra la flotante, si la hay).

### Ejemplo
Topología: LAN-A `192.168.1.0/24` — R1 — `10.0.12.0/30` — R2 — `10.0.23.0/30` — R3 — LAN-C `192.168.3.0/24`, y R1 con salida a internet por `203.0.113.1`.

Diseño típico de "routers de extremo con ruta por defecto":

```text
! R3: solo conoce su LAN y el enlace con R2; todo lo demás, hacia R2
R3(config)# ip route 0.0.0.0 0.0.0.0 10.0.23.1

! R2: está en medio; necesita rutas a ambas LAN y por defecto hacia R1
R2(config)# ip route 192.168.1.0 255.255.255.0 10.0.12.1
R2(config)# ip route 192.168.3.0 255.255.255.0 10.0.23.2
R2(config)# ip route 0.0.0.0 0.0.0.0 10.0.12.1

! R1: hacia la LAN-C y el enlace R2-R3, y por defecto a internet
R1(config)# ip route 192.168.3.0 255.255.255.0 10.0.12.2
R1(config)# ip route 10.0.23.0 255.255.255.252 10.0.12.2
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

(En R1, `10.0.12.2` es R2. En R2, `.1` del primer enlace es R1 y `.2` del segundo es R3.)

Respaldo de internet por un segundo proveedor solo si cae el primero:

```text
R1(config)# ip route 0.0.0.0 0.0.0.0 198.51.100.1 250
R1# show ip route static
R1# show ip route 0.0.0.0
```

Para que el respaldo funcione cuando el proveedor falla "a medias" (la interfaz sigue arriba pero no hay internet), se combina con **IP SLA** y `track`, que retira la ruta principal si deja de responder un ping de prueba.

Practica en el [[Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer]].

### Cuándo usarlo y cuándo no
Úsalas en redes pequeñas o en **redes stub** (con una sola salida), para la ruta por defecto hacia el proveedor, para respaldos flotantes y para casos puntuales donde quieres control total. En redes con muchos routers o caminos redundantes, usa un protocolo dinámico como OSPF: mantener decenas de estáticas a mano es propenso a errores y no reacciona a fallas en saltos intermedios.

### Errores comunes
- **Olvidar la ruta de regreso**.
- **Escribir el siguiente salto equivocado** (una IP propia o una que no está en un enlace directo).
- **Flotante con AD menor** que la principal: se convierte en principal.
- **Usar solo interfaz de salida en Ethernet**: genera ARP por cada destino y depende de proxy ARP.

### Para profundizar
- [RFC 1812: requisitos para routers IPv4](https://www.rfc-editor.org/rfc/rfc1812)
- [Cisco: Configure a Next Hop IP Address for Static Routes](https://www.cisco.com/c/en/us/support/docs/dial-access/floating-static-route/118263-technote-nexthop-00.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- R1 (LAN 192.168.1.0/24) — 10.0.12.0/30 — R2 — 10.0.23.0/30 — R3 (LAN 192.168.3.0/24). R1 es .1 y R2 .2 en el primer enlace; R2 .1 y R3 .2 en el segundo. Escribe las estáticas mínimas en R2.
> ```
> ip route 192.168.1.0 255.255.255.0 10.0.12.1
> ip route 192.168.3.0 255.255.255.0 10.0.23.2
> ```

> [!question]- En la misma topología, ¿qué ruta única basta en R3?
> Una ruta por defecto: `ip route 0.0.0.0 0.0.0.0 10.0.23.1`.

> [!question]- Agrega en R1 un respaldo a 192.168.3.0/24 por un enlace directo a R3 (10.0.13.2) que solo se use si falla el principal.
> `ip route 192.168.3.0 255.255.255.0 10.0.13.2 5` (AD 5 > 1 de la estática principal).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuándo conviene una ruta estática?
> En redes pequeñas o de un solo camino (*stub*), para la ruta por defecto hacia el ISP o como respaldo. No se adaptan solas a fallas.

> [!question]- Escribe una ruta a 192.168.3.0/24 por el siguiente salto 10.0.12.2 y una ruta por defecto hacia 203.0.113.1.
> ```
> ip route 192.168.3.0 255.255.255.0 10.0.12.2
> ip route 0.0.0.0 0.0.0.0 203.0.113.1
> ```

> [!question]- ¿Qué es una ruta estática flotante?
> Una estática con distancia administrativa mayor que la ruta principal (p. ej. `ip route 0.0.0.0 0.0.0.0 198.51.100.1 200`); solo entra en la tabla si la principal desaparece.

> [!question]- ¿Qué pasa con la ruta de regreso si solo configuras la ida?
> El ping falla: el destino recibe la solicitud pero su router no sabe cómo volver. Cada router del camino necesita ruta hacia ambas redes.

## Práctica

- [[Lab 1 - Rutas estáticas y OSPF de área única en Packet Tracer]]

