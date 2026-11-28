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
# FTP, SFTP y TFTP

Parte de: [[19-Redes/04 Servicios de red/_Índice Servicios de red|Servicios de red]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Son tres protocolos para **transferir archivos** que suenan parecido pero funcionan de forma muy distinta:
- **FTP** (File Transfer Protocol): el clásico, sobre TCP, con usuario y contraseña **sin cifrar**.
- **SFTP** (SSH File Transfer Protocol): transferencia de archivos **dentro de una sesión SSH**, cifrada.
- **TFTP** (Trivial File Transfer Protocol): mínimo, sobre UDP, **sin autenticación**; se usa sobre todo para cargar imágenes y configuraciones en equipos de red y para arranque por red (PXE).

### Cómo funciona
| Característica | FTP | SFTP | TFTP |
|---|---|---|---|
| Transporte y puerto | TCP 21 (control) + datos (20 en activo o puerto dinámico en pasivo) | TCP 22 (el de SSH) | UDP 69 |
| Autenticación | Usuario/contraseña en texto claro | SSH: contraseña o llave | Ninguna |
| Cifrado | No | Sí | No |
| Funciones | Listar, subir, bajar, renombrar, borrar | Igual que FTP y más | Solo leer y escribir un archivo |
| Uso típico hoy | Sistemas heredados | Intercambio seguro con proveedores, servidores | IOS, teléfonos IP, PXE |

**FTP activo y pasivo**: FTP usa dos conexiones, una de **control** (comandos, puerto 21) y otra de **datos** para cada transferencia o listado.
- En **modo activo**, el cliente le dice al servidor "conéctate a mí en este puerto" (comando `PORT`) y es el **servidor** quien abre la conexión de datos desde su puerto 20 hacia el cliente. Si el cliente está detrás de NAT o de un firewall, esa conexión entrante no tiene a dónde llegar o se bloquea; además, la IP privada viaja dentro del comando y NAT tendría que reescribirla.
- En **modo pasivo** (`PASV`), el servidor indica un puerto y es el **cliente** quien abre la conexión de datos. Funciona mejor detrás de NAT, pero el firewall del servidor debe permitir el rango de puertos pasivos. Los firewalls modernos tienen inspección FTP para manejar esto.

**SFTP no es FTPS**: SFTP es un subsistema de SSH (un protocolo diferente). **FTPS** es FTP clásico protegido con **TLS** (explícito en el puerto 21 con `AUTH TLS`, o implícito en el 990) y conserva el problema de las dos conexiones. Ambos cifran, pero no son compatibles entre sí.

### Ejemplo
Respaldar la configuración de un router a un servidor TFTP del laboratorio y copiar una imagen:

```text
R1# copy running-config tftp:
Address or name of remote host []? 192.168.99.50
Destination filename [r1-confg]? r1-2026-11-10.cfg
!!
R1# copy tftp: flash:
R1# show flash:
```

Alternativa segura con SCP (copia sobre SSH), que IOS soporta como cliente:

```text
R1# copy running-config scp://respaldo@192.168.99.50/r1-2026-11-10.cfg
```

SFTP desde tu PC (Windows 10/11 y Linux traen el cliente OpenSSH):

```bash
sftp despliegue@sftp.proveedor.mx
sftp> put reporte-noviembre.xlsx
sftp> ls
```

Para Wee.Reporteria, si un cliente exige recibir los Excel por "FTP", ofrécele SFTP o FTPS; nunca FTP plano con credenciales reales.

### Cuándo usarlo y cuándo no
- **SFTP**: tu opción por defecto para transferir archivos entre sistemas.
- **FTPS**: cuando el otro lado solo soporta FTP pero acepta TLS.
- **TFTP**: solo en redes de gestión controladas, para equipos que no soportan otra cosa (arranque, teléfonos, imágenes de IOS en el laboratorio).
- **FTP plano**: evítalo; si es inevitable, que sea en una red aislada y con usuarios sin privilegios.

### Errores comunes
- **Decir que SFTP y FTPS son lo mismo**.
- **Usar TFTP a través de internet**: sin autenticación, cualquiera puede leer o escribir.
- **Abrir solo el puerto 21** para FTP pasivo y olvidar el rango de datos.
- **Dejar la contraseña de FTP en scripts**: guárdala como secreto (Azure Key Vault, variables secretas de Azure DevOps).

### Para profundizar
- [RFC 959: File Transfer Protocol](https://www.rfc-editor.org/rfc/rfc959)
- [RFC 1350: TFTP revisión 2](https://www.rfc-editor.org/rfc/rfc1350)
- [RFC 4217: FTP sobre TLS (FTPS)](https://www.rfc-editor.org/rfc/rfc4217)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Compara FTP, SFTP y TFTP.
> **FTP**: TCP 21 (control) y datos aparte, con usuario y contraseña en texto claro. **SFTP**: transferencia sobre SSH (TCP 22), cifrada. **TFTP**: UDP 69, sin autenticación; se usa para respaldar configuraciones o imágenes en redes de laboratorio.

> [!question]- ¿SFTP y FTPS son lo mismo?
> No. **SFTP** es un subsistema de SSH. **FTPS** es FTP con TLS.

> [!question]- ¿Por qué el FTP activo tiene problemas con NAT y firewalls?
> En modo activo el **servidor** abre la conexión de datos hacia el cliente, y el NAT o firewall del cliente la bloquea. El modo pasivo hace que el cliente abra ambas conexiones.

## Práctica

- [[Lab 1 - SSH y respaldo de configuración en Packet Tracer]]

