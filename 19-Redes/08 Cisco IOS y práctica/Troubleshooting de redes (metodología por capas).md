---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Troubleshooting de redes (metodología por capas)

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **troubleshooting** (solución de problemas) de redes es el proceso sistemático para encontrar y corregir la causa de una falla. Sin método, se cambia "lo primero que se ocurra" y se pierde tiempo o se crean problemas nuevos. Las metodologías basadas en el [[Modelo OSI]] dan un orden lógico: revisar cada capa y descartar.

### Cómo funciona
**Proceso general**:
1. **Definir el problema**: qué falla, desde dónde, desde cuándo, a quién afecta, qué cambió.
2. **Recopilar información**: síntomas, `show`, logs, capturas.
3. **Analizar y formular hipótesis**.
4. **Probar** la hipótesis (una cosa a la vez).
5. **Resolver** y verificar que el síntoma desapareció sin efectos secundarios.
6. **Documentar** la causa, la solución y los cambios.

**Métodos por capas**:
- **De abajo hacia arriba** (bottom-up): empezar en capa 1 y subir. Lento pero exhaustivo; ideal cuando no hay pistas o el problema es nuevo.
- **De arriba hacia abajo** (top-down): empezar por la aplicación. Útil cuando sospechas del servicio (un solo sitio web falla).
- **Divide y vencerás**: empezar en una capa intermedia (por ejemplo, `ping`, capa 3); si funciona, el problema está arriba; si no, abajo.
- **Seguir el camino** (follow the path): recorrer el trayecto del paquete equipo por equipo con traceroute y tablas de rutas.
- **Sustitución**: cambiar un componente sospechoso por uno conocido que funciona (cable, puerto, equipo).
- **Comparación**: comparar con un equipo o configuración que sí funciona.

### Ejemplo
**"Un usuario no navega"**. Secuencia desde su PC con Windows:

```text
1. ipconfig /all            ¿Tiene IP válida, máscara, gateway y DNS? (169.254 = sin DHCP)
2. ping 127.0.0.1           ¿Funciona la pila TCP/IP local?
3. ping <su propia IP>      ¿La interfaz está activa?
4. ping <gateway>           ¿Llega a su red local? (capas 1-3 locales)
5. ping 8.8.8.8             ¿Sale a internet? (rutas, NAT, firewall)
6. nslookup google.com      ¿Resuelve nombres? (DNS)
7. tracert -d 8.8.8.8       Si 5 falla, ¿en qué salto se corta?
8. Test-NetConnection sitio.com -Port 443   ¿Llega al servicio?
```

Si el paso 4 falla, el problema es local (cable, puerto, VLAN, IP o gateway mal configurados). Si 5 falla pero 4 no, está en el router, NAT o proveedor. Si 5 funciona pero 6 no, es DNS. Si todo funciona pero el sitio no carga, es la aplicación, el proxy o el navegador.

**Comandos de IOS por capa**:

| Capa | Comandos |
|---|---|
| 1 Física | `show interfaces status`, `show interfaces` (CRC, colisiones), `show ip interface brief` |
| 2 Enlace | `show mac address-table`, `show vlan brief`, `show interfaces trunk`, `show spanning-tree`, `show port-security`, `show cdp neighbors` |
| 3 Red | `show ip route`, `ping`, `traceroute`, `show ip arp`, `show ip ospf neighbor`, `show access-lists` |
| 4 y más | `show ip nat translations`, `telnet <ip> <puerto>` para probar puertos, `show ip dhcp binding`, `debug` con cuidado |

**Por qué documentar cada prueba y cambio**: para poder **revertir** si algo empeora, para que otra persona continúe sin repetir pruebas, para encontrar patrones en incidentes recurrentes y para construir una base de conocimiento. Anota hora, comando, resultado y conclusión, igual que en tu bitácora de laboratorios (ver [[Laboratorios de Packet Tracer (bitácora)]]).

### Cuándo usarlo y cuándo no
Usa bottom-up ante fallas totales o desconocidas; divide y vencerás cuando tienes experiencia y una pista; top-down cuando falla un servicio específico y otros funcionan. En incidentes graves, prioriza **restaurar el servicio** (por ejemplo, revertir el último cambio) y después busca la causa raíz.

### Errores comunes
- **Cambiar varias cosas a la vez**: no sabrás cuál lo arregló (o lo empeoró).
- **Saltarse la capa 1** por "obvia".
- **Concluir con un solo ping fallido** sin considerar firewalls que bloquean ICMP.
- **No verificar la ruta de regreso**.

### Para profundizar
- [Cisco: Troubleshooting Methodology (Internetwork Troubleshooting Handbook)](https://www.cisco.com/en/US/docs/internetworking/troubleshooting/guide/tr1901.html)
- [RFC 1122: requisitos de hosts](https://www.rfc-editor.org/rfc/rfc1122)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué métodos de troubleshooting por capas existen?
> **De abajo hacia arriba** (capa 1 primero), **de arriba hacia abajo** (desde la aplicación), **divide y vencerás** (empieza en capa 3 con ping) y **seguir la ruta** del tráfico. También comparar con un equipo que sí funciona.

> [!question]- Un usuario no navega. ¿Qué secuencia de pruebas haces desde su PC?
> 1) `ipconfig` (IP, máscara, gateway, DNS válidos). 2) ping al gateway. 3) ping a una IP externa (8.8.8.8). 4) `nslookup` de un nombre. 5) `tracert` para ver dónde se corta.

> [!question]- ¿Qué comandos de IOS usas por capa?
> Capa 1-2: `show interfaces`, `show cdp neighbors`, `show vlan brief`, `show interfaces trunk`. Capa 3: `show ip interface brief`, `show ip route`, `ping`, `traceroute`. Servicios: `show ip nat translations`, `show access-lists`.

> [!question]- ¿Por qué documentar cada prueba y cambio?
> Para no repetir pruebas, poder revertir y dejar una solución reutilizable; además ayuda a encontrar la causa raíz y no solo el síntoma.

## Práctica

- [[Lab 2 - Wireshark capturando ARP, ICMP y DNS]]

