---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Redes
tags: [redes]
---
# PAT (sobrecarga de NAT)

Parte de: [[NAT y PAT]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**PAT** (Port Address Translation), también llamado **NAT overload** o NAPT, traduce **muchas direcciones privadas a una sola IP pública** (o a pocas) distinguiendo cada conexión por el **número de puerto**. Es lo que hace el router de tu casa y casi cualquier router de oficina: decenas o cientos de equipos navegan a la vez con una sola IP pública.

### Cómo funciona
1. La PC `192.168.1.20` abre una conexión desde su puerto `51000` a `20.42.65.92:443`.
2. El router cambia el origen a su IP pública, por ejemplo `203.0.113.2`, y, si hace falta, también el **puerto origen** (por ejemplo a `1025`) para que la combinación sea única.
3. Guarda en su tabla: `192.168.1.20:51000 ↔ 203.0.113.2:1025`.
4. Cuando llega la respuesta a `203.0.113.2:1025`, busca la entrada y la devuelve a `192.168.1.20:51000`.
5. Otra PC que también usa el puerto `51000` recibirá otro puerto público. Con unos 65,000 puertos por IP y por protocolo, una sola dirección soporta miles de conexiones simultáneas.

Para protocolos sin puertos, como ICMP, el router usa el identificador de la consulta de eco de forma equivalente.

Variantes de configuración en IOS:
- **Con la IP de la interfaz outside**: `ip nat inside source list 1 interface g0/1 overload`. Ideal cuando el proveedor te da una IP dinámica.
- **Con un pool**: `ip nat inside source list 1 pool PUBLICAS overload`, si tienes varias públicas.

### Ejemplo
Oficina con VLAN 10 (`192.168.10.0/24`) y VLAN 20 (`192.168.20.0/24`) en router on a stick, saliendo por G0/1 con IP del proveedor:

```text
R1(config)# ip access-list standard NAT-LAN
R1(config-std-nacl)# permit 192.168.10.0 0.0.0.255
R1(config-std-nacl)# permit 192.168.20.0 0.0.0.255
R1(config-std-nacl)# exit
R1(config)# ip nat inside source list NAT-LAN interface g0/1 overload
R1(config)# interface g0/0.10
R1(config-subif)# ip nat inside
R1(config-subif)# interface g0/0.20
R1(config-subif)# ip nat inside
R1(config-subif)# interface g0/1
R1(config-if)# ip nat outside
R1(config-if)# exit
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
R1(config)# end
R1# show ip nat translations
Pro  Inside global      Inside local        Outside local      Outside global
tcp  203.0.113.2:1024   192.168.10.11:50433 20.42.65.92:443    20.42.65.92:443
tcp  203.0.113.2:50434  192.168.20.15:50434 140.82.112.3:443   140.82.112.3:443
icmp 203.0.113.2:1      192.168.10.11:1     8.8.8.8:1          8.8.8.8:1
```

Fíjate: IOS intenta conservar el puerto original si está libre (segunda línea) y solo lo cambia si hay conflicto.

**PAT configurado pero nadie sale**: revisa en este orden:
1. **Interfaces**: ¿todas las inside (incluidas las subinterfaces) tienen `ip nat inside` y la de salida `ip nat outside`?
2. **ACL**: ¿permite exactamente las redes internas, con wildcard correcta? Mira `show access-lists` y si sus contadores suben.
3. **Ruta por defecto** hacia el proveedor: sin ella, el router ni siquiera intenta enviar el paquete por la interfaz outside.
Después, `show ip nat statistics` y `debug ip nat` para confirmar.

Practica en el [[Lab 1 - DHCP y NAT-PAT en Packet Tracer]].

### Cuándo usarlo y cuándo no
Úsalo para dar salida a internet a cualquier LAN con IPv4 privada. No sirve para publicar servidores (las conexiones entrantes no tienen traducción previa); para eso, NAT estático o mapeo de puertos. Ten presente que tu proveedor puede estar haciendo otro PAT encima (CGNAT), lo que impide conexiones entrantes aunque configures port forwarding.

### Errores comunes
- **Olvidar `overload`**: con `interface` sin `overload` solo un equipo saldría a la vez.
- **Subinterfaces sin `ip nat inside`**.
- **ACL con máscara en vez de wildcard**.
- **Sin ruta por defecto**.

### Para profundizar
- [RFC 3022: NAT tradicional (NAPT)](https://www.rfc-editor.org/rfc/rfc3022)
- [RFC 4787: requisitos de comportamiento de NAT para UDP](https://www.rfc-editor.org/rfc/rfc4787)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es PAT?
> Muchos hosts internos comparten **una** IP pública; el router distingue cada conexión por el **puerto** de origen.

> [!question]- Configura PAT con la IP de la interfaz G0/1.
> ```
> access-list 1 permit 192.168.1.0 0.0.0.255
> ip nat inside source list 1 interface g0/1 overload
> ```

> [!question]- Error común: PAT configurado pero nadie sale. ¿Qué tres cosas revisas?
> 1) `ip nat inside`/`outside` en las interfaces correctas. 2) Que la ACL cubra la red interna (wildcard correcta). 3) Que exista una ruta por defecto hacia el ISP.

## Práctica

- [[Lab 1 - DHCP y NAT-PAT en Packet Tracer]]

