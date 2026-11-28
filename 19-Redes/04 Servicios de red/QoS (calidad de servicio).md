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
# QoS (calidad de servicio)

Parte de: [[19-Redes/04 Servicios de red/_Índice Servicios de red|Servicios de red]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**QoS** (Quality of Service) es el conjunto de técnicas para **dar trato diferenciado al tráfico** cuando un enlace se congestiona: priorizar lo sensible al retardo (voz, video interactivo), garantizar un mínimo a lo importante (aplicaciones de negocio) y limitar lo que puede esperar (descargas, respaldos). QoS no crea ancho de banda: decide **quién espera y quién se descarta** cuando no alcanza.

### Cómo funciona
Problemas que ataca: **retardo** (delay), **variación del retardo** (jitter), **pérdida** de paquetes y ancho de banda insuficiente. Para voz se suele recomendar (guía común de Cisco) menos de 150 ms de retardo en un sentido, menos de 30 ms de jitter y menos de 1% de pérdida.

Etapas típicas, en orden:
1. **Clasificación**: identificar el tráfico (por ACL, puertos, aplicación con NBAR, VLAN de voz).
2. **Marcado**: escribir una marca en el paquete para que los demás equipos no tengan que reclasificar. En capa 2, **CoS** (3 bits PCP de la etiqueta 802.1Q, valores 0 a 7). En capa 3, **DSCP** (6 bits del campo ToS de IP, 64 valores).
3. **Frontera de confianza** (trust boundary): decidir en qué punto se aceptan las marcas recibidas (por ejemplo, confiar en el teléfono IP, no en la PC).
4. **Encolamiento y planificación** (queuing): cuando la interfaz se llena, ordenar las colas. **LLQ** (cola de baja latencia, estricta) para voz y **CBWFQ** (ancho de banda garantizado por clase) para el resto.
5. **Evitar la congestión**: **WRED** descarta de forma anticipada y selectiva paquetes de menor prioridad para que TCP reduzca su ritmo antes de que la cola se llene.
6. **Policing y shaping**: el *policing* descarta o remarca lo que excede una tasa; el *shaping* lo retiene en un búfer y lo suaviza (común en la salida hacia un proveedor que te contrata menos que la velocidad física del puerto).

**Marcas DSCP comunes**:

| Tráfico | DSCP | Valor decimal |
|---|---|---|
| Voz | **EF** | 46 |
| Videoconferencia | AF41 | 34 |
| Señalización de llamadas | CS3 | 24 |
| Datos de negocio | AF21 | 18 |
| Mejor esfuerzo | DF (0) | 0 |
| Masivo (respaldos) | CS1 | 8 |

### Ejemplo
Política MQC en el router de una sucursal: voz con prioridad estricta del 20% y el resto con su parte, aplicada en la salida WAN:

```text
R1(config)# class-map match-any VOZ
R1(config-cmap)# match dscp ef
R1(config-cmap)# class-map match-any VIDEO
R1(config-cmap)# match dscp af41
R1(config-cmap)# exit
R1(config)# policy-map WAN-SALIDA
R1(config-pmap)# class VOZ
R1(config-pmap-c)# priority percent 20
R1(config-pmap-c)# class VIDEO
R1(config-pmap-c)# bandwidth percent 25
R1(config-pmap-c)# class class-default
R1(config-pmap-c)# fair-queue
R1(config-pmap-c)# exit
R1(config-pmap)# exit
R1(config)# interface g0/1
R1(config-if)# service-policy output WAN-SALIDA
R1(config-if)# end
R1# show policy-map interface g0/1
```

**¿Sirve QoS si el enlace nunca se congestiona?** Casi no: las colas solo actúan cuando hay más tráfico del que la interfaz puede enviar. Clasificar y marcar sí conviene para estar listo, pero si nunca hay cola, todos salen de inmediato. El problema es que las congestiones suelen ser **microrráfagas** de milisegundos que no se ven en promedios de 5 minutos.

### Cuándo usarlo y cuándo no
Úsala en enlaces WAN e internet con voz o video, en la salida hacia proveedores con velocidad contratada menor a la física, y en Wi-Fi saturado. En la LAN con enlaces gigabit sobrados basta marcar y confiar correctamente. No intentes "arreglar" con QoS un enlace permanentemente insuficiente: ahí lo que toca es más ancho de banda.

### Errores comunes
- **Confiar en las marcas de cualquier PC**: un usuario podría marcar todo como EF.
- **Dar prioridad estricta a demasiado tráfico**: la cola de prioridad acapara el enlace y el resto se queda sin servicio.
- **Configurar QoS solo en un extremo**: el sentido contrario sigue sin control.
- **Aplicar la política en la interfaz equivocada** (entrada en lugar de salida).

### Para profundizar
- [RFC 2474: definición del campo DS (DSCP)](https://www.rfc-editor.org/rfc/rfc2474)
- [RFC 4594: guía de clases de servicio DiffServ](https://www.rfc-editor.org/rfc/rfc4594)
- [RFC 3246: Expedited Forwarding](https://www.rfc-editor.org/rfc/rfc3246)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Para qué sirve QoS?
> Da trato preferente a cierto tráfico (voz, video) cuando hay congestión: controla retardo, variación del retardo (jitter) y pérdida.

> [!question]- ¿Cuáles son las etapas típicas de QoS?
> **Clasificación** y **marcado** (CoS en capa 2, DSCP en capa 3) → **encolamiento** (p. ej. LLQ/CBWFQ) → **policing** (descartar o remarcar el exceso) o **shaping** (retrasarlo en un búfer).

> [!question]- ¿Qué marca DSCP se usa normalmente para voz?
> **EF** (Expedited Forwarding), valor 46.

> [!question]- ¿QoS sirve si el enlace nunca se congestiona?
> Casi no: QoS administra la congestión, no crea ancho de banda. Solo actúa cuando hay más tráfico que capacidad.

