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
# IP pública

Parte de: [[Direcciones públicas y privadas]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **IP pública** es una dirección IPv4 (o IPv6) **única a nivel mundial** y enrutable en internet. Es la que identifica a un servidor, un firewall o el router de tu casa ante el resto del mundo. Los routers de internet saben cómo llegar a ella porque el bloque que la contiene se anuncia por BGP. No se puede usar libremente: se asigna de forma jerárquica.

### Cómo funciona
**Quién asigna**:
1. **IANA** (función de la ICANN) administra el espacio global y lo reparte en bloques grandes.
2. Los cinco **RIR** (Registros Regionales de Internet) los distribuyen por región: ARIN (Norteamérica), **LACNIC** (Latinoamérica y el Caribe, incluido México), RIPE NCC (Europa y Medio Oriente), APNIC (Asia-Pacífico) y AFRINIC (África).
3. Los RIR asignan a **proveedores** (ISP), nubes y organizaciones grandes; en algunos países hay registros nacionales intermedios.
4. El proveedor te da a ti una IP pública, normalmente **dinámica** (cambia de vez en cuando) en planes caseros y **fija** en planes empresariales.

El espacio IPv4 se agotó en los RIR hace años; por eso las IP públicas IPv4 se reutilizan con NAT, se compran en mercados de transferencia y los proveedores recurren a **CGNAT**.

**CGNAT** (Carrier-Grade NAT): el proveedor no te da una IP pública, sino una del rango compartido `100.64.0.0/10`, y hace NAT en su red hacia un pool de públicas compartidas entre muchos clientes. Problemas que causa:
- No puedes recibir conexiones entrantes: abrir puertos en tu router no sirve (ni para un servidor casero, ni para cámaras, ni para algunos juegos).
- Varios clientes comparten la misma IP pública: si uno abusa, los servicios pueden bloquear a todos.
- Algunas VPN y aplicaciones peer-to-peer fallan.
Las soluciones son pedir una IP pública al proveedor (a veces con costo), usar IPv6 o un túnel saliente (como un servicio de túnel o una VPN hacia la nube).

### Ejemplo
Tu PC tiene `192.168.1.20`: esa **no** es tu IP pública; es privada (RFC 1918). Tu IP pública está en la interfaz WAN del módem o la ves desde un servicio externo:

```bash
curl https://api.ipify.org
```

Si la WAN del módem muestra, por ejemplo, `100.72.15.4` pero `api.ipify.org` dice otra dirección, estás detrás de CGNAT.

En Azure, una IP pública es un recurso que asocias a un balanceador, una VM o un firewall:

```bash
az network public-ip create --resource-group rg-dionisio --name pip-api \
  --sku Standard --allocation-method Static
```

En un router Cisco de borde, la IP pública suele estar en la interfaz que da al proveedor y se usa como dirección inside global para PAT:

```text
R1(config)# interface g0/1
R1(config-if)# ip address 203.0.113.2 255.255.255.252
R1(config-if)# ip nat outside
```

### Cuándo usarlo y cuándo no
Necesitas IP pública para publicar servicios (sitios, APIs, VPN, correo) o para que algo en internet inicie conexiones hacia ti. Para el resto de los equipos, usa privadas con NAT. Incluso para publicar, es común tener la IP pública solo en el firewall o balanceador y traducir hacia servidores con IP privada (NAT estático o proxy inverso).

### Errores comunes
- **Confundir la IP de tu PC con tu IP pública**.
- **Abrir puertos en el router y esperar acceso desde afuera** estando detrás de CGNAT.
- **Usar direcciones de documentación** (`203.0.113.0/24`) en una red real: no te pertenecen.
- **Publicar servicios sin firewall** solo porque "tienen IP pública fija".

### Para profundizar
- [LACNIC](https://www.lacnic.net/)
- [IANA: asignación de números](https://www.iana.org/numbers)
- [RFC 6598: espacio compartido para CGNAT](https://www.rfc-editor.org/rfc/rfc6598)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Quién asigna las IP públicas?
> IANA las reparte a los registros regionales (para México y Latinoamérica, **LACNIC**), y estos a los proveedores de internet.

> [!question]- Tu PC tiene 192.168.1.20. ¿Esa es tu IP pública?
> No: es privada. Tu IP pública la tiene tu router o tu proveedor; la ves consultando un servicio externo.

> [!question]- ¿Qué es CGNAT y qué problema causa?
> El proveedor comparte una IP pública entre muchos clientes (usa el rango `100.64.0.0/10` hacia ti). No puedes abrir puertos ni publicar un servidor desde casa.

