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
# NTP

Parte de: [[19-Redes/04 Servicios de red/_Índice Servicios de red|Servicios de red]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**NTP** (Network Time Protocol) sincroniza los relojes de los equipos de la red con una fuente de tiempo confiable, con precisión de milisegundos en una LAN. Parece un detalle, pero sin la hora correcta los **logs** de distintos equipos no se pueden correlacionar, los **certificados** parecen vencidos o aún no válidos, los **tokens** como JWT fallan por `exp` y `nbf`, Kerberos (Active Directory) rechaza autenticaciones y los análisis forenses pierden valor.

### Cómo funciona
- Usa **UDP 123**.
- Organiza las fuentes en una jerarquía de **estratos (stratum)**:
  - **Stratum 0**: la referencia física (reloj atómico, GPS); no está en la red.
  - **Stratum 1**: servidores conectados directamente a una referencia stratum 0.
  - **Stratum 2**: sincronizan con un stratum 1, y así sucesivamente.
  - El máximo útil es **15**; **16** significa "no sincronizado".
  Un número menor indica estar más cerca de la fuente, no necesariamente más preciso, pero es lo que NTP prefiere.
- El cliente consulta a uno o varios servidores, mide el retardo de ida y vuelta, calcula el desfase y corrige el reloj poco a poco para no provocar saltos.
- Se recomienda configurar **al menos tres o cuatro servidores** para que el algoritmo descarte uno que dé la hora mal.
- Puede autenticarse con llaves para que nadie inyecte una hora falsa. **NTS** (Network Time Security) es la versión moderna con TLS.

### Ejemplo
Router de borde como cliente de servidores públicos y como servidor para el resto de la red:

```text
R1(config)# clock timezone CST -6 0
R1(config)# ntp server 0.pool.ntp.org
R1(config)# ntp server 1.pool.ntp.org
R1(config)# ntp server time.cloudflare.com prefer
R1(config)# ntp update-calendar
R1(config)# service timestamps log datetime msec localtime show-timezone
R1(config)# end
R1# show ntp status
Clock is synchronized, stratum 3, reference is 162.159.200.1
R1# show ntp associations
  address         ref clock       st   when   poll reach  delay  offset   disp
*~162.159.200.1   10.21.8.4        3     22     64   377  12.3    0.41    0.9
 ~45.79.1.70      ...
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured
R1# show clock detail
```

El asterisco indica el servidor elegido; `reach 377` (octal) significa que las últimas ocho consultas respondieron. Para usar nombres, el router necesita DNS (`ip name-server`). Los switches de la red apuntan al router: `ntp server 10.99.0.1`. Si el router no tiene salida a internet, se puede convertir en fuente con `ntp master 4`, aunque su hora será la de su propio reloj.

México usa desde finales de 2022 horario fijo sin cambio de verano en la mayor parte del país; el centro está en UTC−6. Revisa la zona de tu región antes de configurarla. Muchas organizaciones prefieren dejar los equipos en **UTC** y convertir al visualizar.

En Windows (cliente de dominio o independiente):

```powershell
w32tm /query /status
w32tm /resync
```

En Azure, las VM sincronizan con la infraestructura de host de Microsoft de forma predeterminada.

### Cuándo usarlo y cuándo no
Siempre: todo router, switch, firewall, servidor y PC debe sincronizar hora. Diseña una jerarquía: dos o tres servidores internos que sincronizan con fuentes externas confiables, y el resto de la red contra los internos. Para aplicaciones que requieren precisión de microsegundos (finanzas, telecom) existe **PTP** (IEEE 1588).

### Errores comunes
- **Un solo servidor NTP**: si falla o da la hora mal, no hay con qué compararlo.
- **Firewall que bloquea UDP 123**.
- **Zona horaria mal configurada** y logs con horas incoherentes; añade `show-timezone` en los timestamps.
- **Confiar en `clock set`** manual en vez de NTP: el reloj se desvía con los días.

### Para profundizar
- [RFC 5905: NTP versión 4](https://www.rfc-editor.org/rfc/rfc5905)
- [RFC 8915: Network Time Security (NTS)](https://www.rfc-editor.org/rfc/rfc8915)
- [NTP Pool Project](https://www.ntppool.org/es/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve NTP?
> Sincroniza la hora de los equipos. Es clave para que los logs se puedan correlacionar, para certificados y para autenticación.

> [!question]- ¿Qué es el stratum?
> La distancia a la fuente de hora de referencia: stratum 0 es el reloj (GPS, atómico), stratum 1 el servidor conectado a él, y así sucesivamente. Menor es mejor.

> [!question]- ¿Cómo configuras y verificas un cliente NTP en IOS?
> `ntp server 192.168.1.10`; verifica con `show ntp status` y `show ntp associations`. NTP usa UDP 123.

