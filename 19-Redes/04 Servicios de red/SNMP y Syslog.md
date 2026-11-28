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
# SNMP y Syslog

Parte de: [[19-Redes/04 Servicios de red/_Índice Servicios de red|Servicios de red]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son los dos pilares clásicos del **monitoreo de red**:
- **SNMP** (Simple Network Management Protocol) permite **consultar y, opcionalmente, modificar** variables de los equipos (uso de CPU, tráfico de cada interfaz, estado de ventiladores) y recibir **notificaciones** (traps o informs) cuando algo pasa.
- **Syslog** es el estándar para **enviar mensajes de registro** (logs) de los equipos a un servidor central, con un nivel de severidad.

Juntos te dicen "cómo está" la red (SNMP) y "qué pasó" (syslog).

### Cómo funciona
**SNMP**:
- Componentes: el **gestor** (NMS, por ejemplo Zabbix, LibreNMS, PRTG), el **agente** en cada equipo y la **MIB**, el árbol de variables identificadas por un **OID** (como `1.3.6.1.2.1.2.2.1.10` para los bytes recibidos de una interfaz).
- Operaciones: `get`, `getnext`, `getbulk`, `set`, y notificaciones `trap` (sin confirmación) o `inform` (confirmadas).
- Puertos: **UDP 161** (consultas al agente) y **UDP 162** (traps hacia el gestor).
- Versiones:
  - **v1 y v2c**: autenticación con una **community string** que viaja en **texto claro**. Cualquiera que la capture puede leer (o, con una comunidad RW, cambiar) la configuración.
  - **v3**: usuarios con **autenticación** (SHA) y **cifrado** (AES); niveles noAuthNoPriv, authNoPriv y **authPriv**. Es la que debes usar.

**Syslog**:
- Puerto **UDP 514** (también puede ir por TCP o con TLS).
- Cada mensaje tiene **facility** (origen lógico) y **severidad** de 0 a 7:

| Nivel | Nombre | Significado |
|---|---|---|
| 0 | emergencies | Sistema inutilizable |
| 1 | alerts | Acción inmediata |
| 2 | critical | Condición crítica |
| 3 | errors | Error |
| 4 | warnings | Advertencia |
| 5 | notifications | Normal pero significativo (interfaz up/down) |
| 6 | informational | Informativo |
| 7 | debugging | Depuración |

Mnemotecnia en inglés para el orden: "**E**very **A**wesome **C**isco **E**ngineer **W**ill **N**eed **I**ce cream **D**aily". Al configurar un nivel, se envían **ese y todos los más graves** (números menores).

### Ejemplo
Monitoreo de R1 con SNMPv3 y envío de logs a un servidor syslog en la VLAN de gestión:

```text
R1(config)# ip access-list standard GESTORES
R1(config-std-nacl)# permit 10.99.0.50
R1(config-std-nacl)# exit
R1(config)# snmp-server group MONITOREO v3 priv read VISTA-LECTURA access GESTORES
R1(config)# snmp-server view VISTA-LECTURA iso included
R1(config)# snmp-server user zabbix MONITOREO v3 auth sha <ClaveAuth> priv aes 128 <ClavePriv>
R1(config)# snmp-server host 10.99.0.50 version 3 priv zabbix
R1(config)# snmp-server enable traps
R1(config)# service timestamps log datetime msec localtime show-timezone
R1(config)# logging host 10.99.0.50
R1(config)# logging trap warnings
R1(config)# logging buffered 16384 informational
R1(config)# end
R1# show logging
R1# show snmp user
```

Un mensaje típico:

```text
*Nov 10 09:41:22.311 CST: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down
```

`%LINEPROTO-5-UPDOWN` = facility, **severidad 5** (notifications) y mnemónico. Con `logging trap warnings` (nivel 4) este mensaje **no** se enviaría al servidor; si te interesa, usa `logging trap notifications`.

Las claves no deben quedarse en documentos ni en el repositorio: guárdalas en un gestor de secretos.

### Cuándo usarlo y cuándo no
SNMP para métricas y alertas de estado (gráficas de tráfico, disponibilidad). Syslog para eventos y auditoría (quién entró, qué cambió, qué interfaz cayó). Ambos con NTP configurado, o los tiempos no cuadrarán. Para métricas de alta frecuencia se usa cada vez más **telemetría por streaming** (gNMI, modelos YANG) en equipos modernos.

### Errores comunes
- **Dejar las comunidades `public` y `private`** por defecto.
- **Usar SNMP v2c con permiso de escritura** en redes no aisladas.
- **No limitar con ACL** qué gestores pueden consultar.
- **Enviar nivel debugging** al servidor en producción: inunda el syslog y la CPU del equipo.

### Para profundizar
- [RFC 3411: arquitectura de SNMP](https://www.rfc-editor.org/rfc/rfc3411)
- [RFC 3414: modelo de seguridad basado en usuarios de SNMPv3](https://www.rfc-editor.org/rfc/rfc3414)
- [RFC 5424: protocolo Syslog](https://www.rfc-editor.org/rfc/rfc5424)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirven SNMP y Syslog?
> **SNMP** permite consultar y monitorear equipos (CPU, interfaces) y recibir alertas (*traps*). **Syslog** envía mensajes de eventos a un servidor central.

> [!question]- ¿Qué puertos usan?
> SNMP: UDP **161** (consultas al agente) y **162** (traps). Syslog: UDP **514**.

> [!question]- ¿Qué versión de SNMP usarías y por qué?
> **SNMPv3**: agrega autenticación y cifrado. v1/v2c usan *community strings* en texto claro.

> [!question]- Ordena los niveles de severidad de syslog.
> 0 emergencies · 1 alerts · 2 critical · 3 errors · 4 warnings · 5 notifications · 6 informational · 7 debugging. `logging trap 4` envía del 0 al 4.

