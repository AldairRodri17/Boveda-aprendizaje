---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# Direcciones MAC

Parte de: [[19-Redes/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **dirección MAC** (Media Access Control) es el identificador de capa 2 de una interfaz de red. Mide **48 bits** (6 bytes) y se escribe en hexadecimal. La usan Ethernet y Wi-Fi para entregar tramas dentro de un mismo segmento local. A diferencia de la IP, la MAC no tiene jerarquía de red: no te dice "dónde" está el equipo, solo "quién" es en el enlace.

### Cómo funciona
**Estructura**:
- Los primeros 24 bits son el **OUI** (Organizationally Unique Identifier), asignado por el IEEE al fabricante.
- Los últimos 24 bits los asigna el fabricante a cada interfaz.

**Formatos de escritura** (es el mismo valor):
- Windows: `3C-52-82-12-34-56`
- Linux y macOS: `3c:52:82:12:34:56`
- Cisco IOS: `3c52.8212.3456` (tres grupos de cuatro dígitos con punto)

**Bits especiales del primer byte**:
- Bit menos significativo (**I/G**): 0 = unicast, 1 = multicast o grupo. Si el primer byte es impar, la MAC es de grupo. `01:00:5E:…` es el prefijo de multicast IPv4 y `33:33:…` el de multicast IPv6.
- Segundo bit menos significativo (**U/L**): 0 = asignada por el fabricante (universal), 1 = administrada localmente. Si el segundo dígito hexadecimal es 2, 6, A o E, es local. Docker (`02:42:…`), las VM y la **MAC aleatoria** de iPhone y Windows usan direcciones locales.

**Broadcast**: `FF:FF:FF:FF:FF:FF`, todos los bits en 1; la reciben todas las interfaces del dominio de broadcast.

**MAC frente a IP**: la MAC es de capa 2, plana y de alcance local; cambia en cada salto porque el router reescribe la trama. La IP es de capa 3, jerárquica (red + host), configurable y se mantiene de extremo a extremo. Para pasar de una a otra se usa [[ARP]].

### Ejemplo
Ver la MAC en tu equipo y en un switch Cisco:

```bash
# Windows
getmac /v
ipconfig /all        # "Dirección física"
# Linux
ip link show
```

```text
SW1# show mac address-table dynamic
Vlan    Mac Address       Type        Ports
  10    3c52.8212.3456    DYNAMIC     Fa0/3
  10    0242.ac11.0002    DYNAMIC     Fa0/4
SW1# show interfaces fa0/1 | include address
  Hardware is Fast Ethernet, address is 0019.e8a2.0b01 (bia 0019.e8a2.0b01)
```

`bia` (burned-in address) es la MAC grabada de fábrica. La segunda entrada, `0242.ac11.0002`, empieza con `02`: segundo bit en 1, así que es local (un contenedor Docker). Para saber el fabricante de un OUI consulta el registro público del IEEE.

Nota práctica: tu iPhone 15 Pro usa por defecto una "dirección Wi-Fi privada" distinta para cada red. Si reservas una IP por MAC en el DHCP de casa, desactiva esa opción para esa red o la reserva no funcionará.

### Cuándo usarlo y cuándo no
Usa la MAC para diagnóstico de capa 2 (¿en qué puerto está este equipo?), para reservas de DHCP, port security y filtrado en Wi-Fi. No la uses como mecanismo de seguridad fuerte: se puede cambiar por software en segundos (MAC spoofing). Para autenticar dispositivos usa 802.1X.

### Errores comunes
- **Creer que la MAC viaja hasta el servidor remoto**: llega solo al router; después cambia.
- **Confundir formatos**: `00-1A-2B-3C-4D-5E` y `001a.2b3c.4d5e` son la misma dirección.
- **Asumir que la MAC es fija**: la aleatorización en móviles y laptops rompe filtros y reservas basados en MAC.
- **Leer mal el bit de multicast**: se mira el primer byte; `01` es multicast, `00` es unicast.

### Para profundizar
- [IEEE Registration Authority: OUI](https://standards.ieee.org/regauth/oui/)
- [RFC 7042: parámetros IANA para IEEE 802 (MAC de multicast)](https://www.rfc-editor.org/rfc/rfc7042)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe `00-1A-2B-3C-4D-5E` en formato Cisco.
> `001a.2b3c.4d5e` (tres grupos de 4 dígitos hexadecimales separados por punto).

> [!question]- ¿La MAC `01:00:5E:00:00:05` es unicast o multicast? ¿Cómo lo sabes?
> **Multicast.** El bit menos significativo del primer byte (I/G) es 1: `0x01` = `0000 0001`. Además `01:00:5E` es el prefijo de multicast IPv4.

> [!question]- ¿La MAC `02:42:AC:11:00:02` es asignada por fabricante o local?
> **Administrada localmente.** El segundo bit menos significativo del primer byte (U/L) es 1: `0x02` = `0000 0010`. Es típica de contenedores Docker y máquinas virtuales.

> [!question]- ¿Cuál es el OUI de `3C:52:82:12:34:56`?
> `3C:52:82` (los primeros 3 bytes). Puedes buscarlo en la lista pública de OUI de IEEE para saber el fabricante.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo está formada una dirección MAC?
> 48 bits (12 dígitos hexadecimales). Los primeros 24 bits son el **OUI** (fabricante) y los últimos 24 los asigna el fabricante a cada tarjeta.

> [!question]- ¿Cuál es la MAC de broadcast?
> `FF:FF:FF:FF:FF:FF`.

> [!question]- ¿Qué diferencia hay entre una MAC y una IP?
> La MAC identifica la interfaz dentro del **segmento local** (capa 2) y cambia en cada salto. La IP identifica al host de **extremo a extremo** (capa 3).

## Práctica

- [[Lab 1 - Encapsulamiento, hub y switch en modo simulación]]

