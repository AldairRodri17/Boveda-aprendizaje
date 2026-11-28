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
# ACLs (estándar y extendidas)

Parte de: [[19-Redes/07 Seguridad de redes/_Índice Seguridad de redes|Seguridad de redes]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **ACL** (Access Control List) es una lista ordenada de reglas `permit` o `deny` que un router (o switch capa 3) usa para **filtrar o clasificar** paquetes. Se aplican a interfaces para permitir o bloquear tráfico, a las líneas VTY para restringir la administración, y también para identificar tráfico en NAT, QoS, VPN o rutas. Hay dos tipos según lo que revisan, [[ACL estándar]] y [[ACL extendida]], y dos formas de escribirlas: numeradas o [[ACL nombrada]].

### Cómo funciona
**Procesamiento**:
1. IOS compara el paquete con las líneas **de arriba hacia abajo**.
2. En la **primera coincidencia** aplica la acción (permit o deny) y **deja de revisar**.
3. Si ninguna línea coincide, aplica el **deny implícito** al final de toda ACL (`deny any`), que no se ve pero existe.

Consecuencias: el **orden importa** (lo específico arriba, lo general abajo) y una ACL con solo líneas `deny` **bloquea todo**, porque lo que no coincide con los deny cae en el deny implícito. Toda ACL de filtrado necesita al menos un `permit`.

**Tipos**:

| Tipo | Revisa | Números |
|---|---|---|
| Estándar | Solo la **IP origen** | 1–99 y 1300–1999 |
| Extendida | Protocolo, IP origen y destino, puertos, flags TCP, ICMP | 100–199 y 2000–2699 |

**Wildcards**: las ACL usan wildcard (máscara invertida): 0 = "debe coincidir", 1 = "no importa". `host 10.1.1.5` equivale a `10.1.1.5 0.0.0.0` y `any` equivale a `0.0.0.0 255.255.255.255`.

**Aplicación**: `ip access-group <acl> in|out` en la interfaz. **Entrada** (in) filtra antes de enrutar; **salida** (out) después de enrutar. Regla: **una ACL por interfaz, por dirección y por protocolo** (IPv4 e IPv6 son independientes): como máximo, una ACL IPv4 de entrada y una de salida en cada interfaz. Si aplicas otra en el mismo sentido, reemplaza a la anterior.

**Ubicación recomendada**:
- **Estándar cerca del destino**: como solo mira el origen, si la pones cerca del origen bloquearías ese tráfico hacia todos los destinos, no solo hacia el que querías proteger.
- **Extendida cerca del origen**: como puede ser precisa (origen, destino y puerto), conviene descartar el tráfico no deseado lo antes posible para que no cruce la red.

El tráfico **generado por el propio router** (sus pings, sus actualizaciones) no es filtrado por las ACL de salida de ese router.

### Ejemplo
Ver, aplicar y diagnosticar:

```text
R1# show access-lists
Extended IP access list 110
    10 permit tcp 192.168.10.0 0.0.0.255 host 192.168.20.15 eq 443 (152 matches)
    20 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255 (8 matches)
    30 permit ip any any (3021 matches)
R1# show ip interface g0/0 | include access list
  Outgoing access list is not set
  Inbound  access list is 110
R1# clear access-list counters 110
```

Los contadores de *matches* te dicen qué líneas se están usando; si una línea que esperabas nunca suma, otra de arriba está ganando.

Practica en el [[Lab 1 - ACLs estándar y extendidas en Packet Tracer]].

### Cuándo usarlo y cuándo no
Úsalas para segmentar entre VLAN, proteger servidores, restringir gestión (VTY, SNMP) y clasificar tráfico para NAT o QoS. No sustituyen a un firewall con estado en el perímetro: no siguen conexiones ni inspeccionan aplicaciones (ver [[Firewalls (stateless, stateful y de nueva generación)]]).

### Errores comunes
- **Olvidar el deny implícito**: ACL solo con deny bloquea todo.
- **Orden incorrecto**: una línea general arriba anula a las específicas.
- **Aplicarla en el sentido equivocado** (in vs out) o en la interfaz equivocada.
- **Editar una ACL numerada "agregando" líneas** esperando que queden en medio: se agregan al final (con las nombradas o con números de secuencia puedes insertar).

### Para profundizar
- [Cisco: Configure Commonly Used IP ACLs](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/26448-ACLsamples.html)
- [Cisco: Security Configuration Guide, Access Control Lists](https://cisco.com/en/US/docs/ios-xml/ios/sec_data_acl/configuration/12-4t/sec-data-acl-12-4t-book.html)

## Subtemas

- [[ACL estándar]]
- [[ACL extendida]]
- [[ACL nombrada]]

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo procesa IOS una ACL?
> De arriba hacia abajo; en la **primera coincidencia** aplica permit o deny y deja de buscar. Al final hay un **deny implícito** para todo lo demás.

> [!question]- ¿Dónde se coloca una ACL estándar y dónde una extendida?
> **Estándar** (solo origen): lo más cerca posible del **destino**. **Extendida** (origen, destino, protocolo, puerto): lo más cerca posible del **origen**.

> [!question]- ¿Cuántas ACL puedes aplicar a una interfaz?
> Una por **interfaz**, por **dirección** (in/out) y por **protocolo** (IPv4/IPv6).

> [!question]- Error común: una ACL con solo líneas `deny`. ¿Qué pasa?
> Bloquea **todo** por el deny implícito. Hay que terminar con un `permit` (p. ej. `permit any` o `permit ip any any`).

## Práctica

- [[Lab 1 - ACLs estándar y extendidas en Packet Tracer]]

## Progreso de los subtemas

```base
filters:
  and:
    - file.inFolder("19-Redes/07 Seguridad de redes/ACLs (estándar y extendidas)")
    - file.name != "ACLs (estándar y extendidas)"
views:
  - type: table
    name: Progreso
    order:
      - file.name
      - progreso
      - nivel
```
