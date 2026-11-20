---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Respaldo de configuraciones

Parte de: [[19-Redes/08 Cisco IOS y práctica/_Índice Cisco IOS y práctica|Cisco IOS y práctica]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Respaldar configuraciones** es guardar copias de la running-config (y la startup-config) de cada router y switch **fuera del propio equipo**, con fecha y control de versiones. Si un equipo falla, se reemplaza o alguien hace un cambio desastroso, el respaldo permite restaurarlo en minutos. Además, comparar versiones te dice exactamente qué cambió y cuándo, igual que el historial de Git en tu código.

### Cómo funciona
**Por qué fuera del equipo**: la startup-config está en la NVRAM del propio equipo; si este se daña, se roba o se reemplaza, esa copia desaparece con él. También necesitas un histórico, no solo la última versión.

**Destinos y protocolos**:
- **TFTP** (UDP 69): el clásico de laboratorio. Simple pero **sin autenticación ni cifrado**.
- **SCP** (sobre SSH, TCP 22) y **SFTP**: cifrados y autenticados. Recomendados.
- **FTP**: con usuario, sin cifrado.
- **Flash local** o USB: útil como copia rápida antes de un cambio, pero no reemplaza a un respaldo externo.
- **Herramientas**: Oxidized o RANCID (libres), o plataformas de gestión que guardan versiones en Git y avisan de cambios.

**Copiar hacia el equipo**: hay una diferencia importante:
- `copy tftp: running-config` **fusiona** (merge) el archivo con la configuración activa: los comandos del archivo se aplican uno a uno encima de lo que ya hay; lo que no esté en el archivo permanece. Puede dejar mezclas inesperadas.
- `copy tftp: startup-config` **reemplaza** la configuración guardada; la nueva se aplica al **reiniciar**. Es la forma limpia de restaurar completo.
- En IOS reciente, `configure replace` reemplaza la running-config de forma atómica sin reiniciar.

**Función `archive`** de IOS: guarda automáticamente una copia en un servidor cada vez que se escribe la configuración o cada cierto tiempo.

### Ejemplo
Respaldo manual a un servidor TFTP del laboratorio y con SCP:

```text
R1# copy running-config tftp:
Address or name of remote host []? 10.99.0.50
Destination filename [r1-confg]? r1-2026-11-10.cfg
!!
1830 bytes copied in 0.5 secs

R1# copy running-config scp://respaldos@10.99.0.50/r1-2026-11-10.cfg
```

Respaldo automático con `archive`:

```text
R1(config)# ip scp server enable
R1(config)# archive
R1(config-archive)# path scp://respaldos:<Clave>@10.99.0.50/$h-$t
R1(config-archive)# write-memory
R1(config-archive)# time-period 1440
```

`$h` es el hostname y `$t` la fecha y hora. `write-memory` respalda cada vez que guardas.

Restauración completa de un equipo de reemplazo:

```text
R1# copy tftp: startup-config
R1# reload
```

Antes de cada cambio importante: `copy running-config flash:antes-cambio.cfg`, y si algo sale mal, `configure replace flash:antes-cambio.cfg`.

En el servidor de respaldos, un script programado puede subir los archivos a un repositorio de Azure DevOps para tener historial y revisión de cambios (sin incluir secretos en texto: revisa que no haya contraseñas tipo 7 ni comunidades SNMP en claro).

Practica con el [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]], que incluye un servidor TFTP.

### Cuándo usarlo y cuándo no
Respalda **siempre**: tras cada cambio y de forma automática diaria. Usa SCP o SFTP en producción y TFTP solo en redes de gestión aisladas o en el laboratorio. Guarda también la imagen de IOS y la licencia del equipo.

### Errores comunes
- **Restaurar con `copy tftp: running-config`** esperando un reemplazo limpio.
- **Respaldos solo en la flash** del propio equipo.
- **TFTP por internet** o en redes de usuarios.
- **Nunca probar la restauración**: un respaldo que no sabes restaurar no sirve.

### Para profundizar
- [Cisco: Configuration Replace and Configuration Rollback](https://www.cisco.com/en/US/docs/ios-xml/ios/config-mgmt/configuration/12-2sx/cm-config-rollback.html)
- [Oxidized (respaldo de configuraciones, código abierto)](https://github.com/ytti/oxidized)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Por qué respaldar configuraciones fuera del equipo?
> Si el equipo falla o alguien borra la configuración, restauras en minutos. También sirve para comparar cambios y auditar.

> [!question]- ¿Cómo respaldas la running-config a un servidor TFTP?
> `copy running-config tftp:` → IP del servidor → nombre de archivo (p. ej. `R1-confg`).

> [!question]- ¿Qué diferencia hay entre `copy tftp: running-config` y `copy tftp: startup-config`?
> A **running-config** se **fusiona** con la configuración actual (no borra líneas que ya existían). A **startup-config** la reemplaza y se aplica al reiniciar.

> [!question]- ¿Qué alternativa más segura existe a TFTP?
> SCP o SFTP (cifrados), o herramientas de respaldo automático como scripts con Netmiko, Ansible o el comando `archive` de IOS.

## Práctica

- [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]]

