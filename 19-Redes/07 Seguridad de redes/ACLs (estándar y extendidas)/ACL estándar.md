---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# ACL estándar

Parte de: [[ACLs (estándar y extendidas)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **ACL estándar** filtra paquetes **solo por la dirección IP origen**. No distingue destino, protocolo ni puerto. Se identifica con números del **1 al 99** o del **1300 al 1999**, o con un nombre. Por su poca precisión se coloca **cerca del destino**. Es muy útil para cosas simples: restringir quién administra un equipo, definir qué redes se traducen con NAT o qué orígenes se aceptan en SNMP.

### Cómo funciona
Sintaxis numerada:

```text
access-list <1-99> {permit | deny} <origen> [wildcard] | host <ip> | any
```

**Wildcard**: la inversa de la máscara (255 menos cada octeto). Ejemplos:

| Red | Máscara | Wildcard |
|---|---|---|
| 192.168.30.0/24 | 255.255.255.0 | 0.0.0.255 |
| 10.50.8.0/21 | 255.255.248.0 | 0.0.7.255 |
| 172.16.40.128/26 | 255.255.255.192 | 0.0.0.63 |
| 10.20.30.64/27 | 255.255.255.224 | 0.0.0.31 |
| Un solo host | 255.255.255.255 | 0.0.0.0 (`host`) |
| Cualquiera | 0.0.0.0 | 255.255.255.255 (`any`) |

La regla de lectura: un 0 en la wildcard obliga a que ese bit coincida; un 1 lo ignora. Así, `10.1.0.0 0.0.255.255` abarca `10.1.0.0` a `10.1.255.255`.

**Aplicación**:
- En una interfaz: `ip access-group <n> in|out`.
- En las líneas VTY (para restringir SSH o Telnet): `access-class <n> in` dentro de `line vty 0 15`. Aquí no importa dónde esté el administrador en la topología: se filtra el acceso al propio router.

### Ejemplo
**Caso 1: proteger la VLAN de Finanzas** (`192.168.30.0/24`, detrás de G0/3 de R1) para que la VLAN de Invitados (`192.168.40.0/24`) no llegue, permitiendo lo demás. Como es estándar, va **cerca del destino**, en la salida hacia Finanzas:

```text
R1(config)# access-list 20 remark Bloquea Invitados hacia Finanzas
R1(config)# access-list 20 deny 192.168.40.0 0.0.0.255
R1(config)# access-list 20 permit any
R1(config)# interface g0/3
R1(config-if)# ip access-group 20 out
```

Si la hubieras puesto en la **entrada** de la interfaz de Invitados, también bloquearías a los invitados hacia internet, que sí se permitía.

**Caso 2: solo la VLAN de gestión puede entrar por SSH al router**:

```text
R1(config)# access-list 5 permit 10.99.0.0 0.0.0.255
R1(config)# line vty 0 15
R1(config-line)# access-class 5 in
R1(config-line)# transport input ssh
```

Con el deny implícito, cualquier otra red es rechazada.

**Caso 3: wildcard para un rango "raro"**, como permitir solo `172.16.40.128/26`:

```text
R1(config)# access-list 7 permit 172.16.40.128 0.0.0.63
```

Verifica rangos con Python antes de escribirlos:

```python
import ipaddress
n = ipaddress.ip_network("172.16.40.128/26")
print(n.hostmask, n[0], n[-1])   # 0.0.0.63 172.16.40.128 172.16.40.191
```

Y en el router:

```text
R1# show access-lists 20
R1# show ip interface g0/3 | include access list
```

### Cuándo usarlo y cuándo no
Úsala cuando solo importa **quién origina**: VTY, NAT, SNMP, distribución de rutas, debug condicionado. Para filtrar por servicio (puerto) o por destino concreto, usa una [[ACL extendida]].

### Errores comunes
- **Colocarla cerca del origen**: bloquea al origen hacia todo.
- **Usar máscara en lugar de wildcard** (`255.255.255.0`): coincide con cosas inesperadas.
- **Olvidar `permit any`** al final cuando solo quieres bloquear algunos.
- **Aplicar `ip access-group` en VTY**: ahí se usa `access-class`.

### Para profundizar
- [Cisco: Configure Commonly Used IP ACLs](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/26448-ACLsamples.html)
- [Cisco Guide to Harden Cisco IOS Devices](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/13608-21.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Wildcard para 192.168.10.0/24.
> `0.0.0.255`

> [!question]- Wildcard para 172.16.32.0/20.
> `0.0.15.255` (255.255.255.255 − 255.255.240.0).

> [!question]- Wildcard para 192.168.1.64/26.
> `0.0.0.63`

> [!question]- ¿Qué abarca `10.1.0.0 0.0.255.255`?
> De `10.1.0.0` a `10.1.255.255` (equivale a 10.1.0.0/16).

> [!question]- ¿Qué abreviaturas equivalen a `0.0.0.0 255.255.255.255` y a `10.1.1.5 0.0.0.0`?
> `any` y `host 10.1.1.5`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué revisa una ACL estándar y qué números usa?
> Solo la **IP de origen**. Números 1–99 y 1300–1999 (o nombre).

> [!question]- Bloquea al host 192.168.10.50 hacia la red de servidores (salida G0/2) y permite lo demás.
> ```
> access-list 1 deny host 192.168.10.50
> access-list 1 permit any
> interface g0/2
>  ip access-group 1 out
> ```

> [!question]- ¿Cómo restringes el acceso por VTY a la red 192.168.20.0/24?
> ```
> access-list 10 permit 192.168.20.0 0.0.0.255
> line vty 0 4
>  access-class 10 in
> ```

## Práctica

- [[Lab 1 - ACLs estándar y extendidas en Packet Tracer]]

