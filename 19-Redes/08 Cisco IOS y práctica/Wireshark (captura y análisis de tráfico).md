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
# Wireshark (captura y análisis de tráfico)

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Wireshark** es el analizador de protocolos de red más usado: **captura** los paquetes que pasan por una interfaz y los **decodifica** capa por capa, mostrando cada campo de Ethernet, IP, TCP, DNS, HTTP, TLS y cientos de protocolos más. Es gratuito y de código abierto, y es la mejor forma de ver con tus propios ojos lo que estudias en teoría (ARP, DHCP, el saludo TCP). Para línea de comandos existen `tshark` y `tcpdump`.

### Cómo funciona
**Captura**: Wireshark pone la tarjeta en modo promiscuo (en cable) y guarda todo lo que la tarjeta recibe. En Windows usa el controlador **Npcap**, que se instala junto con Wireshark.

**Dos tipos de filtro**:
- **Filtro de captura** (sintaxis BPF, la de tcpdump): se define **antes** de capturar y decide qué se guarda. Reduce el tamaño del archivo, pero lo que no capturaste no lo puedes analizar después. Ejemplos: `host 192.168.1.10`, `port 53`, `tcp port 443`, `not arp`.
- **Filtro de visualización** (sintaxis propia de Wireshark): se aplica **después**, sobre lo capturado, y solo oculta o muestra. Es mucho más expresivo. Ejemplos:

```text
arp
icmp
dns
dhcp
ip.addr == 192.168.1.10
ip.src == 10.0.0.5 && tcp.port == 1433
tcp.flags.syn == 1 && tcp.flags.ack == 0
tcp.analysis.retransmission
http.request.method == "POST"
tls.handshake.type == 1
!(arp || dns)
```

**Paneles**: la lista de paquetes, el detalle por capas (expande "Ethernet II", "Internet Protocol", etc.) y los bytes en hexadecimal. Herramientas útiles: *Follow TCP Stream*, *Statistics → Conversations* y *Expert Information*.

**¿Por qué en un switch no ves el tráfico unicast entre otras dos PCs?** Porque el switch reenvía cada trama unicast **solo** al puerto del destino (ver [[Cómo aprende un switch (tabla MAC)]]). A tu puerto solo llegan tus tramas, los broadcasts y algunos multicast. Para ver el tráfico de otros equipos necesitas un **puerto espejo (SPAN)** en el switch, un **TAP** de red, capturar en el propio equipo de interés, o en Wi-Fi una tarjeta en modo monitor. Con un hub se veía todo, por eso se usaban antes.

```text
SW1(config)# monitor session 1 source interface fa0/5 both
SW1(config)# monitor session 1 destination interface fa0/24
```

### Ejemplo
Practica estos ejercicios en tu PC (ver también el [[Lab 2 - Wireshark capturando ARP, ICMP y DNS]]):
1. **ARP**: inicia la captura con filtro de visualización `arp`, ejecuta `arp -d *` (como administrador) y luego `ping 192.168.1.1`. Verás el Request en broadcast y el Reply unicast.
2. **DNS**: filtro `dns`, ejecuta `nslookup dev.azure.com`. Observa la consulta (tipo A), la respuesta y el puerto UDP 53.
3. **TCP**: filtro `tcp.port == 443 && tcp.flags.syn == 1`, abre un sitio y localiza el SYN, SYN-ACK y ACK.
4. **Tu API local**: captura en la interfaz *Adapter for loopback traffic capture* con `tcp.port == 5000` mientras pruebas un endpoint HTTP de Itzamna.Dionisio en desarrollo; verás la petición completa porque no está cifrada (en HTTPS solo verías TLS).

Desde la línea de comandos:

```bash
tshark -i Ethernet -f "port 53" -c 20
tcpdump -i eth0 -n host 10.0.0.5 and tcp port 1433 -w captura.pcap
```

**Consideraciones legales y éticas**: captura solo en redes **propias o donde tienes autorización explícita**. Las capturas pueden contener contraseñas, cookies de sesión, datos personales y datos de clientes; en México, la protección de datos personales está regulada y una empresa puede sancionar o denunciar capturas no autorizadas. Guarda los archivos `.pcap` como información sensible, borra lo que no necesites y no los subas a sitios públicos sin anonimizar.

### Cuándo usarlo y cuándo no
Úsalo para aprender protocolos, diagnosticar problemas difíciles (retransmisiones, MTU, handshakes TLS fallidos, DHCP que no responde) y validar reglas de firewall. Para monitoreo continuo usa NetFlow, SNMP o herramientas de observabilidad; Wireshark es para análisis puntual.

### Errores comunes
- **Confundir filtro de captura y de visualización** (escribir `ip.addr ==` en el filtro de captura).
- **Esperar ver el tráfico de otros** en un puerto de switch normal.
- **Capturar durante horas sin filtro**: archivos gigantes imposibles de analizar.
- **Compartir capturas con datos sensibles**.

### Para profundizar
- [Guía de usuario de Wireshark](https://www.wireshark.org/docs/wsug_html_chunked/)
- [Referencia de filtros de visualización de Wireshark](https://www.wireshark.org/docs/dfref/)
- [Página de manual de los filtros de Wireshark](https://www.wireshark.org/docs/man-pages/wireshark-filter.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre filtro de captura y filtro de visualización?
> **Captura** (sintaxis BPF, p. ej. `port 53`): decide qué se guarda. **Visualización** (p. ej. `dns`): oculta lo que no quieres ver sin perder datos.

> [!question]- Escribe filtros de visualización para ARP, ICMP, DNS y una IP.
> `arp` · `icmp` · `dns` · `ip.addr == 192.168.1.10`. Para una consulta específica: `dns.qry.name == "example.com"`.

> [!question]- ¿Por qué en un switch no ves el tráfico unicast entre otras dos PCs?
> El switch solo te envía lo que va a tu MAC, broadcasts y multicasts. Para ver el resto se usa un puerto espejo (SPAN) o un TAP.

> [!question]- ¿Qué consideración legal o ética hay?
> Captura solo en redes propias o con autorización: el tráfico puede contener datos personales y contraseñas.

## Práctica

- [[Lab 2 - Wireshark capturando ARP, ICMP y DNS]]

