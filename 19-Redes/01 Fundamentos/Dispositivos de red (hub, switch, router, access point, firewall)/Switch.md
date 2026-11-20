---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Switch

Parte de: [[Dispositivos de red (hub, switch, router, access point, firewall)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un **switch** es un dispositivo de **capa 2** que conecta equipos de la misma LAN y reenvía cada trama **solo por el puerto donde está el destino**. Para lograrlo aprende qué MAC está detrás de cada puerto y guarda esa información en su tabla MAC (también llamada tabla CAM). Es el dispositivo más común en cualquier red cableada.

### Cómo funciona
Por cada trama que recibe, el switch hace:
1. **Aprender**: lee la MAC **origen** y anota "esta MAC está en este puerto y esta VLAN".
2. **Decidir** con la MAC **destino**:
   - Si la conoce, **reenvía** (forward) solo por ese puerto.
   - Si es desconocida, broadcast o multicast sin control, **inunda** (flood) por todos los puertos de la VLAN menos el de entrada.
   - Si el destino está en el mismo puerto de entrada, **filtra** (descarta).
3. Las entradas dinámicas envejecen: si una MAC no envía tráfico en **300 segundos** (valor predeterminado en Cisco), se borra.

El detalle de este proceso está en [[Cómo aprende un switch (tabla MAC)]].

Ventajas frente a un [[Hub]]:
- **Cada puerto es su propio dominio de colisión**; con full dúplex, las colisiones desaparecen.
- **Ancho de banda dedicado** por puerto: 24 PCs pueden transmitir a la vez a 1 Gbps cada una.
- **Más privacidad**: el tráfico unicast solo llega al destino.

Lo que **no** hace un switch capa 2 es separar broadcasts: todos los puertos de una misma VLAN forman **un dominio de broadcast**. Para dividirlo usas VLAN (ver [[VLANs]]) y, para comunicar VLAN entre sí, un router o un switch capa 3.

Métodos de reenvío: **store-and-forward** (recibe la trama completa y valida el FCS; el más común), **cut-through** (empieza a reenviar al leer la MAC destino; menor latencia, propaga tramas dañadas) y **fragment-free** (espera los primeros 64 bytes).

### Ejemplo
Comandos básicos en un Cisco 2960 para conocer al switch:

```text
SW1# show mac address-table
SW1# show mac address-table interface fa0/3
SW1# show mac address-table aging-time
SW1# show interfaces status
SW1# show vlan brief
```

Escenario: una impresora no imprime. Buscas su MAC en el switch:

```text
SW1# show mac address-table address 0021.5a11.2233
Vlan    Mac Address       Type        Ports
  20    0021.5a11.2233    DYNAMIC     Fa0/15
```

Está en VLAN 20, pero las PCs del área están en VLAN 10: la impresora quedó en la VLAN equivocada. Corriges el puerto:

```text
SW1(config)# interface fa0/15
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10
```

### Cuándo usarlo y cuándo no
Usa switches **administrables** en cualquier red de oficina: permiten VLAN, port security, STP, monitoreo y SPAN. Los no administrables están bien para casa o un escritorio con pocos equipos. Si necesitas enrutar entre VLAN con mucho tráfico interno, usa un switch capa 3 (ver [[Switch capa 3 (SVI)]]).

### Errores comunes
- **Creer que el switch separa dominios de broadcast**: solo si hay VLAN distintas.
- **Confundir "inundar" con "broadcast"**: el switch inunda tramas unicast desconocidas, pero no las convierte en broadcast.
- **Conectar dos switches con dos cables sin STP**: bucle y tormenta de broadcast.
- **Olvidar que la tabla MAC tiene límite**: un ataque de MAC flooding la llena y el switch se comporta como hub; port security lo mitiga.

### Para profundizar
- [IEEE 802.1 Working Group (puentes y VLAN)](https://www.ieee802.org/1/)
- [Cisco Catalyst 2960 Software Configuration Guide](https://www.cisco.com/c/en/us/support/switches/catalyst-2960-series-switches/products-installation-and-configuration-guides-list.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo decide un switch por qué puerto enviar una trama?
> Busca la MAC destino en su **tabla MAC**: si la conoce, la envía solo por ese puerto; si no, la inunda por todos los puertos de la VLAN menos el de entrada.

> [!question]- ¿Qué ventaja da un switch frente a un hub?
> Cada puerto es su propio dominio de colisión, permite full-duplex y el tráfico unicast solo llega al destino.

> [!question]- ¿Un switch separa dominios de broadcast?
> **No** (a menos que uses VLANs). Un broadcast llega a todos los puertos de la misma VLAN.

## Práctica

- [[Lab 1 - Encapsulamiento, hub y switch en modo simulación]]

