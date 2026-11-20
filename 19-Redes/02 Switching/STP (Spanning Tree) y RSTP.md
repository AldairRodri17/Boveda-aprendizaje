---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# STP (Spanning Tree) y RSTP

Parte de: [[19-Redes/02 Switching/_Índice Switching|Switching]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**STP** (Spanning Tree Protocol, IEEE 802.1D) evita **bucles de capa 2** en redes con enlaces redundantes entre switches. Construye un árbol lógico sin ciclos: elige un switch raíz y **bloquea** los puertos que cerrarían un bucle, dejándolos listos para activarse si falla un enlace. **RSTP** (Rapid STP, 802.1w, hoy integrado en 802.1D-2004) hace lo mismo pero converge en segundos en lugar de 30 a 50. Cisco usa por defecto PVST+ (una instancia por VLAN) y en muchos equipos Rapid PVST+.

### Cómo funciona
**El problema**: la trama Ethernet no tiene TTL. Con un bucle, un broadcast circula para siempre y se multiplica (**tormenta de broadcast**), las tablas MAC se vuelven inestables y aparecen tramas duplicadas. La red cae en segundos.

**El algoritmo**, con BPDU intercambiadas entre switches:
1. **Elegir el root bridge**: gana el **Bridge ID más bajo**. El BID es prioridad (16 bits) + MAC. La prioridad por defecto es **32768** y en PVST+ se le suma el ID de VLAN (*extended system ID*), por eso verás 32769 en la VLAN 1. La prioridad configurable va de **0 a 61440 en múltiplos de 4096**. Si empatan en prioridad, gana la **MAC más baja**.
2. **Elegir el root port** en cada switch no raíz: el puerto con menor **costo acumulado** hasta la raíz. Desempates: menor BID del vecino, luego menor ID de puerto del vecino.
3. **Elegir el designated port** en cada segmento: el que ofrece el menor costo hacia la raíz. Todos los puertos del root bridge son designados.
4. Los puertos restantes se **bloquean** (802.1D) o quedan como **alternate/backup** (RSTP).

**Costos** (método corto de 802.1D-1998, el que usa Cisco por defecto): 10 Mbps = 100, 100 Mbps = 19, 1 Gbps = 4, 10 Gbps = 2.

**Roles de puerto en RSTP**: root, designated, **alternate** (camino de respaldo a la raíz), **backup** (respaldo de un designado en el mismo segmento) y disabled. **Estados**: en 802.1D son blocking, listening, learning, forwarding y disabled; RSTP los simplifica a **discarding, learning y forwarding**.

**STP frente a RSTP**: 802.1D tarda 30 a 50 segundos en reconvergir porque espera temporizadores (max age 20 s, forward delay 15 s ×2). RSTP usa un intercambio proposal/agreement entre vecinos y puertos alternate precalculados, y converge en uno o dos segundos.

**PortFast y BPDU Guard**: PortFast lleva un puerto de acceso directo a forwarding (sin esperar), ideal para PCs y servidores. BPDU Guard apaga (err-disabled) un puerto PortFast si recibe una BPDU, para que nadie conecte un switch no autorizado y altere el árbol. También existen **Root Guard** (impide que un puerto se convierta en root port por BPDU superiores) y **Loop Guard**.

### Ejemplo
Tres switches en triángulo: SW1 (32768, MAC `0001.0001.0001`), SW2 (32768, MAC `0002.0002.0002`), SW3 (32768, MAC `0003.0003.0003`), todos con enlaces Gigabit.
- Root: **SW1** (MAC más baja con igual prioridad).
- En SW2 y SW3, el root port es el enlace directo a SW1 (costo 4).
- En el enlace SW2–SW3, ambos ofrecen costo 4 a la raíz; gana como designado el de menor BID (SW2). El puerto de SW3 hacia SW2 queda **bloqueado/alternate**.

Quieres que el root sea el núcleo SW3. Configura:

```text
SW3(config)# spanning-tree mode rapid-pvst
SW3(config)# spanning-tree vlan 1,10,20 root primary
! o bien: spanning-tree vlan 1,10,20 priority 4096
SW2(config)# spanning-tree vlan 1,10,20 root secondary
SW1(config)# interface range fa0/1 - 20
SW1(config-if-range)# spanning-tree portfast
SW1(config-if-range)# spanning-tree bpduguard enable
SW1# show spanning-tree vlan 10
SW1# show spanning-tree summary
```

Practica en el [[Lab 2 - STP y EtherChannel en Packet Tracer]].

### Cuándo usarlo y cuándo no
Siempre activo en redes con switches (incluso sin redundancia aparente: protege de cables mal conectados). Elige el root a propósito (el núcleo), usa RSTP y aplica PortFast + BPDU Guard en puertos de usuario. Para aprovechar enlaces paralelos en lugar de bloquearlos, agrúpalos con [[EtherChannel (LACP y PAgP)]].

### Errores comunes
- **Dejar que la elección de raíz sea "al azar"**: puede ganar un switch viejo del rincón con MAC baja.
- **Prioridad que no es múltiplo de 4096**: IOS la rechaza.
- **PortFast en un puerto hacia otro switch**: riesgo de bucle temporal.
- **Desactivar STP "porque bloquea un puerto"**: ese bloqueo es lo que evita el bucle.

### Para profundizar
- [IEEE 802.1 Working Group (802.1D/802.1Q)](https://www.ieee802.org/1/)
- [Cisco: Understand Rapid Spanning Tree Protocol (802.1w)](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/24062-146.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- SW-A: prioridad 32768, MAC 000A.0000.0003. SW-B: 32768, MAC 000A.0000.0001. SW-C: 32768, MAC 000A.0000.0009. ¿Quién es root?
> **SW-B**: misma prioridad, MAC más baja.

> [!question]- Ahora en SW-C configuras `spanning-tree vlan 1 priority 4096`. ¿Quién es root?
> **SW-C**: la prioridad se compara antes que la MAC y 4096 < 32768.

> [!question]- Con SW-C como root: SW-A llega a SW-C directo por FastEthernet (costo 19) o vía SW-B por dos enlaces Gigabit (4 + 4). ¿Cuál es el root port de SW-A?
> El puerto hacia **SW-B**: costo 8 < 19. El enlace directo A–C queda bloqueado en el lado de SW-A (alternate).

> [!question]- ¿Qué valores de prioridad son válidos?
> Múltiplos de **4096** entre 0 y 61440.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué problema resuelve STP?
> Evita **bucles de capa 2** en topologías redundantes (tormentas de broadcast, tablas MAC inestables, tramas duplicadas) bloqueando puertos de forma lógica.

> [!question]- ¿Cómo se elige el root bridge?
> El switch con el **Bridge ID más bajo**: primero la prioridad (por defecto 32768 + VLAN) y, si empatan, la MAC más baja.

> [!question]- ¿Qué roles de puerto existen en RSTP?
> **Root port** (mejor camino al root), **designated** (reenvía en cada segmento), **alternate** (respaldo del root port, bloqueado) y **backup**.

> [!question]- ¿Qué diferencia práctica hay entre STP 802.1D y RSTP 802.1w?
> 802.1D tarda 30–50 s en converger (estados listening/learning y temporizadores). RSTP converge en segundos con negociación propuesta/acuerdo y puertos alternos listos.

> [!question]- ¿Para qué sirven PortFast y BPDU Guard?
> **PortFast** pasa un puerto de acceso a forwarding de inmediato. **BPDU Guard** lo pone en *err-disabled* si recibe una BPDU (alguien conectó un switch). Se usan juntos solo en puertos hacia hosts.

## Práctica

- [[Lab 2 - STP y EtherChannel en Packet Tracer]]

