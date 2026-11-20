---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Redes
herramienta: Wireshark (en tu PC con Windows)
tags: [redes, laboratorio]
---
# Lab 2 - Wireshark capturando ARP, ICMP y DNS

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

**Cubre:** [[Wireshark (captura y análisis de tráfico)]] · [[Troubleshooting de redes (metodología por capas)]] · [[Encapsulamiento y PDUs]] · [[Ethernet y tramas]] · [[DNS en la red (resolución, registros y servidores)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Capturar en tu propia PC tráfico real de ARP, ICMP y DNS con Wireshark, filtrarlo e interpretar sus campos.

## Requisitos

- Wireshark instalado (incluye Npcap en Windows). Captura solo en tu red o con permiso.
- Una terminal **como administrador** (para borrar la caché ARP).
- Conoce tu IP y tu puerta de enlace: `ipconfig`.

## Pasos

1. Abre Wireshark y elige tu interfaz activa (Wi-Fi o Ethernet; la que muestra la gráfica de tráfico).
2. Opcional: pon el filtro de **captura** `arp or icmp or port 53` para guardar solo lo necesario.
3. Inicia la captura y en la terminal ejecuta:
   ```
   arp -d *
   ping -n 4 <tu puerta de enlace>
   ipconfig /flushdns
   nslookup example.com
   ping -n 2 example.com
   tracert -d -h 5 8.8.8.8
   ```
4. Detén la captura y guarda el archivo `.pcapng` en la carpeta de este laboratorio.
5. Aplica los filtros de visualización `arp`, `icmp` y `dns`, uno por uno, y contesta los criterios.
6. Usa **Statistics → Conversations** y **Follow → UDP Stream** en un paquete DNS.

## Criterios de terminado

- [ ] Con `arp` encontré un **request** (opcode 1) a `ff:ff:ff:ff:ff:ff` preguntando por la IP del gateway y su **reply** (opcode 2) con la MAC.
- [ ] Con `icmp` encontré echo request (**tipo 8**) y echo reply (**tipo 0**) y anoté el TTL de la solicitud y de la respuesta.
- [ ] Con `icmp` vi mensajes **Time-to-live exceeded** (tipo 11) generados por el `tracert`.
- [ ] Con `dns` encontré una consulta **A** de `example.com` por UDP 53 y su respuesta con la misma *Transaction ID*.
- [ ] Puedo señalar en un paquete el encabezado Ethernet, el IP y el de la capa superior.
- [ ] Guardé el `.pcapng` junto a esta nota.

## Solución

> [!success]- Qué deberías ver en ARP
> - Request: `Who has 192.168.x.1? Tell 192.168.x.y`, destino Ethernet `ff:ff:ff:ff:ff:ff`, opcode 1.
> - Reply: `192.168.x.1 is at aa:bb:cc:...`, unicast hacia tu MAC, opcode 2.
> - Si no aparece el request, tu caché no se borró (falta abrir la terminal como administrador) o el equipo ya había renovado la entrada.

> [!success]- Qué deberías ver en ICMP
> - `Echo (ping) request` tipo 8 y `Echo (ping) reply` tipo 0, con el mismo identificador y número de secuencia.
> - El TTL de la respuesta depende del sistema que responde y de cuántos saltos recorrió (valores iniciales típicos: 64, 128 o 255).
> - `tracert` en Windows envía ICMP con TTL 1, 2, 3...; cada router intermedio responde **Time-to-live exceeded** (tipo 11).

> [!success]- Qué deberías ver en DNS
> - Consulta: `Standard query 0x1234 A example.com`, UDP, puerto destino 53.
> - Respuesta: `Standard query response 0x1234 A example.com A <IP>`; el puerto origen es 53.
> - Filtro útil: `dns.qry.name == "example.com"`. Si no ves consultas del navegador, puede estar usando DNS cifrado (DoH); por eso se usa `nslookup`.

> [!success]- Filtros de captura frente a visualización
> `arp or icmp or port 53` es sintaxis de **captura** (BPF). En la barra de visualización se usa `arp || icmp || dns`.

## Notas personales

_Qué me costó, errores que cometí, dudas._
