---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# IP privada (RFC 1918)

Parte de: [[Direcciones públicas y privadas]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Las **direcciones privadas** son tres bloques de IPv4 reservados por el **RFC 1918** para uso interno en cualquier organización, sin pedir permiso a nadie. No son enrutables en internet: los proveedores las filtran. Para salir a internet, los equipos con IP privada dependen de NAT. Son la base de casi todas las LAN del mundo y de las redes virtuales en la nube.

### Cómo funciona
Los tres rangos:

| Rango | Prefijo | Desde – hasta | Direcciones |
|---|---|---|---|
| Bloque de 24 bits | 10.0.0.0/8 | 10.0.0.0 – 10.255.255.255 | 16,777,216 |
| Bloque de 20 bits | 172.16.0.0/12 | 172.16.0.0 – 172.31.255.255 | 1,048,576 |
| Bloque de 16 bits | 192.168.0.0/16 | 192.168.0.0 – 192.168.255.255 | 65,536 |

Ojo con el segundo bloque: es **/12**, así que solo va de **172.16** a **172.31**. Una dirección como `172.32.1.1` o `172.15.0.1` es **pública**. Es una trampa clásica de examen.

Como nadie controla su uso, **dos empresas pueden usar 10.0.0.0/8 al mismo tiempo** sin problema mientras sus redes no se conecten. El problema aparece cuando se fusionan, se conectan por VPN o integran con la nube: si ambas tienen `10.1.0.0/16`, los routers no saben a cuál ir. Se resuelve renumerando o con NAT entre ellas (más complejo).

Elegir rangos con cuidado:
- Las redes caseras usan casi siempre `192.168.0.0/24` y `192.168.1.0/24`. Si tu empresa usa esas mismas, los empleados que entran por VPN desde casa tendrán conflictos.
- Proveedores de nube y herramientas de contenedores usan sus propios rangos por defecto (Docker, por ejemplo, crea redes en `172.17.0.0/16` y siguientes): verifica que no choquen con tu red corporativa.

### Ejemplo
Plan de direccionamiento para la empresa con on-premises y Azure sin traslapes:

```text
10.0.0.0/16    Oficina central (VLAN de usuarios, servidores, voz)
10.1.0.0/16    Sucursal Guadalajara
10.50.0.0/16   VNet de Azure producción (Itzamna.Dionisio, Pochteca.Nomisma)
10.51.0.0/16   VNet de Azure desarrollo
172.16.0.0/12  Reservado para laboratorios y contenedores
```

Comprobar si una dirección es privada con Python:

```python
import ipaddress
red_privada = [ipaddress.ip_network(n) for n in ("10.0.0.0/8", "172.16.0.0/12", "192.168.0.0/16")]
for a in ["172.31.255.1", "172.32.1.1", "192.168.100.7", "10.255.0.1"]:
    ip = ipaddress.ip_address(a)
    print(a, any(ip in n for n in red_privada))
# 172.31.255.1 True · 172.32.1.1 False · 192.168.100.7 True · 10.255.0.1 True
```

En Cisco, una ACL estándar que identifica todo el espacio RFC 1918 (útil para NAT o para filtrar tráfico entrante falsificado en el borde):

```text
R1(config)# ip access-list standard RFC1918
R1(config-std-nacl)# permit 10.0.0.0 0.255.255.255
R1(config-std-nacl)# permit 172.16.0.0 0.15.255.255
R1(config-std-nacl)# permit 192.168.0.0 0.0.255.255
```

### Cuándo usarlo y cuándo no
Usa RFC 1918 para todo lo interno. En Azure, AWS y GCP las redes virtuales también suelen usar estos rangos. No uses rangos públicos ajenos "porque nadie los usa" dentro de tu red: cuando necesites llegar a ese servicio real en internet, no podrás.

### Errores comunes
- **Pensar que 172.16.0.0/16 es todo el rango privado de la "clase B"**: es 172.16.0.0/12.
- **Escoger rangos que chocan con VPN, socios o la nube**.
- **Creer que una IP privada es inaccesible desde fuera**: con NAT estático o port forwarding sí lo es.
- **Olvidar el rango de CGNAT** (`100.64.0.0/10`), que no es RFC 1918 pero tampoco es público normal.

### Para profundizar
- [RFC 1918: direcciones para redes privadas](https://www.rfc-editor.org/rfc/rfc1918)
- [Microsoft Learn: planear redes virtuales de Azure](https://learn.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuáles son los tres rangos privados?
> `10.0.0.0/8` · `172.16.0.0/12` (172.16.0.0 – 172.31.255.255) · `192.168.0.0/16`

> [!question]- ¿172.32.1.1 es privada?
> **No.** El rango 172 privado termina en 172.31.255.255.

> [!question]- ¿Pueden dos empresas usar 10.0.0.0/8 al mismo tiempo?
> Sí, porque esas direcciones no salen a internet. El problema aparece si conectas ambas redes (VPN): se enciman y hay que renumerar o usar NAT.

