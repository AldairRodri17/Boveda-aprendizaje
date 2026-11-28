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
# ACL nombrada

Parte de: [[ACLs (estándar y extendidas)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **ACL nombrada** es una ACL estándar o extendida identificada con un **nombre** descriptivo en lugar de un número (`ip access-list extended DEV-A-SERVIDORES`). Además de ser más legible, se configura en un submodo donde cada línea tiene un **número de secuencia**, lo que permite **insertar y borrar líneas individuales** sin rehacer toda la lista. Es la forma recomendada de escribir ACL hoy.

### Cómo funciona
- Se crea con `ip access-list standard <nombre>` o `ip access-list extended <nombre>` y se entra al submodo (`config-std-nacl` o `config-ext-nacl`).
- Cada línea recibe un número de secuencia automático: **10, 20, 30…** (de diez en diez).
- Para **insertar** una línea entre dos existentes, escribe el número: `15 permit ...`.
- Para **borrar** una línea, `no 20`.
- Para **reordenar** con espacio de sobra: `ip access-list resequence <nombre> 10 10`.
- Las ACL numeradas modernas también aceptan números de secuencia si entras con `ip access-list extended 110`, pero los nombres siguen siendo más claros.
- Se aplica igual que una numerada: `ip access-group <nombre> in|out` en la interfaz, o `access-class <nombre> in` en las VTY.
- `remark` permite comentar dentro de la ACL.

Buenas prácticas de nombres: usa mayúsculas, indica la función y el sentido, por ejemplo `VENTAS-IN`, `DMZ-A-LAN`, `GESTION-VTY`.

### Ejemplo
ACL extendida nombrada para la entrada de la VLAN de Ventas (`192.168.50.0/24`):

```text
R1(config)# ip access-list extended VENTAS-IN
R1(config-ext-nacl)# remark Ventas hacia CRM y bloqueo a Finanzas
R1(config-ext-nacl)# permit tcp 192.168.50.0 0.0.0.255 host 192.168.20.40 eq 443
R1(config-ext-nacl)# deny ip 192.168.50.0 0.0.0.255 192.168.30.0 0.0.0.255
R1(config-ext-nacl)# permit ip 192.168.50.0 0.0.0.255 any
R1(config-ext-nacl)# exit
R1(config)# interface g0/0.50
R1(config-subif)# ip access-group VENTAS-IN in
R1(config-subif)# end
R1# show access-lists VENTAS-IN
Extended IP access list VENTAS-IN
    10 permit tcp 192.168.50.0 0.0.0.255 host 192.168.20.40 eq 443
    20 deny ip 192.168.50.0 0.0.0.255 192.168.30.0 0.0.0.255
    30 permit ip 192.168.50.0 0.0.0.255 any
```

Ahora el equipo de Ventas necesita también imprimir en `192.168.30.25` (puerto 9100), que está en la red de Finanzas que bloqueamos. La línea debe ir **antes** del deny (secuencia 20):

```text
R1(config)# ip access-list extended VENTAS-IN
R1(config-ext-nacl)# 15 permit tcp 192.168.50.0 0.0.0.255 host 192.168.30.25 eq 9100
R1(config-ext-nacl)# end
```

Y si después se decide quitar el bloqueo a Finanzas:

```text
R1(config)# ip access-list extended VENTAS-IN
R1(config-ext-nacl)# no 20
```

Guarda la ACL en tu repositorio de configuraciones (sin secretos) para revisar los cambios con historial, como haces con el código.

ACL estándar nombrada para VTY:

```text
R1(config)# ip access-list standard GESTION-VTY
R1(config-std-nacl)# permit 10.99.0.0 0.0.0.255
R1(config-std-nacl)# exit
R1(config)# line vty 0 15
R1(config-line)# access-class GESTION-VTY in
```

### Cuándo usarlo y cuándo no
Prefiérelas siempre: son autodocumentadas y editables. Las numeradas aparecen en material antiguo y en el examen, por eso debes saber leer ambas. Para ACL de IPv6 **solo** existen nombradas (`ipv6 access-list NOMBRE`, aplicada con `ipv6 traffic-filter`). En equipos con muchas ACL, una convención de nombres consistente te permite buscar en los respaldos de configuración y entender en segundos qué hace cada lista, igual que un buen nombre de método en tu código de C#.

### Errores comunes
- **Insertar sin número**: la línea se agrega al final, después del permit general, y nunca coincide.
- **Escribir el nombre distinto** al aplicarla (mayúsculas o guiones): IOS acepta aplicar una ACL que no existe y no filtra nada.
- **Olvidar el permit final** en una ACL de solo bloqueos.
- **Usar espacios en el nombre**.

### Para profundizar
- [Cisco: Security Configuration Guide, Access Control Lists](https://cisco.com/en/US/docs/ios-xml/ios/sec_data_acl/configuration/12-4t/sec-data-acl-12-4t-book.html)
- [Cisco: Configure Commonly Used IP ACLs](https://www.cisco.com/c/en/us/support/docs/ip/access-lists/26448-ACLsamples.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué ventaja tiene una ACL nombrada?
> Nombre descriptivo y edición por **número de secuencia**: puedes insertar o borrar líneas sin reescribir toda la ACL.

> [!question]- Inserta una línea en la posición 15 y borra la 20 de la ACL `VENTAS-IN`.
> ```
> ip access-list extended VENTAS-IN
>  15 permit tcp 192.168.10.0 0.0.0.255 any eq 443
>  no 20
> ```

> [!question]- ¿Cómo se aplica a una interfaz?
> `ip access-group VENTAS-IN in` (igual que una numerada, usando el nombre).

## Práctica

- [[Lab 1 - ACLs estándar y extendidas en Packet Tracer]]

