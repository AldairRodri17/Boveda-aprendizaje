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
# NAT dinámico

Parte de: [[NAT y PAT]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **NAT dinámico** traduce las direcciones internas a direcciones de un **pool de IPs públicas**, asignándolas **uno a uno** según se necesiten. Cuando un equipo interno inicia tráfico, el router le presta una dirección libre del pool mientras dure la traducción; al expirar, la dirección vuelve al pool. A diferencia del estático, el mapeo no es fijo y no permite conexiones iniciadas desde afuera de forma confiable.

### Cómo funciona
1. Se define un **pool** de direcciones públicas: `ip nat pool <nombre> <inicio> <fin> netmask <máscara>` (o `prefix-length`).
2. Se define una **ACL** que dice qué direcciones internas pueden traducirse.
3. Se une todo: `ip nat inside source list <acl> pool <nombre>`.
4. Se marcan las interfaces inside y outside.
5. Cuando un equipo permitido por la ACL envía tráfico, el router toma la **siguiente IP libre** del pool y crea una traducción. Mientras exista, todo el tráfico de ese equipo usa esa IP pública.
6. La traducción expira tras un tiempo de inactividad (por defecto **24 horas** para traducciones simples en IOS; ajustable con `ip nat translation timeout`), o se puede limpiar con `clear ip nat translation`.

**¿Qué pasa cuando se agota el pool?** Si todas las direcciones están prestadas, el siguiente equipo que intente salir **no obtiene traducción** y sus paquetes se descartan (en `show ip nat statistics` aumentan los *misses* y aparecen fallos de asignación). Ese equipo no tendrá internet hasta que se libere alguna dirección. Por eso, con un pool pequeño y muchos usuarios, NAT dinámico simple casi no se usa; se agrega `overload` al pool para convertirlo en PAT.

### Ejemplo
LAN `192.168.1.0/24` y un bloque público de once direcciones, `203.0.113.10` a `203.0.113.20`:

```text
R1(config)# ip nat pool PUBLICAS 203.0.113.10 203.0.113.20 netmask 255.255.255.0
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# ip nat inside source list 1 pool PUBLICAS
R1(config)# interface g0/0
R1(config-if)# ip nat inside
R1(config-if)# interface g0/1
R1(config-if)# ip nat outside
R1(config-if)# end
R1# show ip nat translations
Pro Inside global     Inside local      Outside local   Outside global
--- 203.0.113.10      192.168.1.20      ---             ---
--- 203.0.113.11      192.168.1.21      ---             ---
R1# show ip nat statistics
Total active translations: 2 (0 static, 2 dynamic; 0 extended)
...
pool PUBLICAS: netmask 255.255.255.0
        start 203.0.113.10 end 203.0.113.20
        type generic, total addresses 11, allocated 2 (18%), misses 0
```

Con once direcciones, solo **once** equipos pueden salir al mismo tiempo. Si el doceavo intenta navegar, verás `misses` aumentando.

Convertirlo en PAT sobre el pool (lo que realmente se usaría):

```text
R1(config)# no ip nat inside source list 1 pool PUBLICAS
R1(config)# ip nat inside source list 1 pool PUBLICAS overload
```

Para practicar en [[Cisco Packet Tracer]], usa un pool chico (dos direcciones) y tres PCs: verás cómo la tercera no puede salir.

### Cuándo usarlo y cuándo no
Es poco común hoy. Tiene sentido cuando un socio exige que cada equipo de tu lado se presente con una IP distinta y tienes suficientes direcciones, o en escenarios de traslape entre redes privadas (traducir a un rango "neutral"). Para salida a internet usa PAT; para publicar servidores, NAT estático o mapeo de puertos.

### Errores comunes
- **Pool más pequeño que los usuarios** sin `overload`.
- **Máscara del pool incorrecta**: IOS la usa para validar el rango.
- **ACL que no coincide** con la red interna (o usar máscara en lugar de wildcard).
- **Esperar conexiones entrantes**: la IP pública de un equipo cambia y solo existe mientras hay traducción.

### Para profundizar
- [RFC 3022: NAT tradicional](https://www.rfc-editor.org/rfc/rfc3022)
- [RFC 2663: terminología de NAT](https://www.rfc-editor.org/rfc/rfc2663)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es NAT dinámico?
> Traduce IPs privadas a un **pool** de públicas, uno a uno, según se necesiten.

> [!question]- Escribe los comandos con un pool 203.0.113.10–20 para la LAN 192.168.1.0/24.
> ```
> ip nat pool PUBLICAS 203.0.113.10 203.0.113.20 netmask 255.255.255.224
> access-list 1 permit 192.168.1.0 0.0.0.255
> ip nat inside source list 1 pool PUBLICAS
> ```

> [!question]- ¿Qué pasa cuando se agota el pool?
> Los siguientes hosts no pueden salir hasta que se libere una traducción. Por eso casi siempre se usa PAT.

