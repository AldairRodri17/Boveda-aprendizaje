---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
contenido: completo
---
# Licencias de Power BI (Free, Pro, Premium por usuario y capacidades)

Parte de: [[17-Power-BI/01 Fundamentos/_Índice Fundamentos|Fundamentos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El licenciamiento de Power BI decide **quién puede crear, compartir y consumir** contenido y **con qué recursos** se ejecuta. Hay dos familias que se combinan:

1. **Licencias por usuario**: se asignan a cada persona en el centro de administración de Microsoft 365.
2. **Capacidades**: un bloque de cómputo dedicado que compra la organización y se asigna a áreas de trabajo.

Lo que puedes hacer depende de las dos: tu licencia y la capacidad del área de trabajo donde vive el contenido.

### Cómo funciona
**Por usuario** (según Microsoft Learn, revisado en noviembre de 2026):

| Licencia | Qué permite |
|---|---|
| **Free** (Fabric gratuita) | Crear informes y paneles para uso personal en *Mi área de trabajo*. No puede compartir ni publicar en áreas de otros. Puede **consumir** contenido compartido solo si está en una capacidad Premium P o Fabric F64 o mayor. |
| **Pro** | Publicar, compartir y colaborar con otros usuarios Pro; consumir lo que otros publiquen. Es la licencia típica de quien crea. |
| **Premium por usuario (PPU)** | Todo lo de Pro más casi todas las funciones Premium (modelos más grandes, más actualizaciones al día, XMLA de lectura/escritura, canalizaciones de implementación) para ese usuario. Lo que vive en un área PPU solo lo pueden ver otros usuarios **PPU**. |

**Capacidades**:

- **Fabric F SKUs** (F2, F4, F8 … F64 y superiores), compradas en Azure con pago por uso o reserva. Sirven para Power BI y para el resto de Fabric. Con **F64 o mayor**, los usuarios Free pueden consumir lo que se les comparte; en **F2–F32** cada lector necesita Pro o PPU.
- **Power BI Premium P SKUs**: Microsoft ya no las vende a clientes nuevos y las está retirando en favor de las F; quien tiene contrato EA vigente puede renovarlas hasta que termine.
- **Embedded (A y EM)**: capacidades para incrustar en aplicaciones propias. No se ven afectadas por el retiro de las P.

Regla práctica: **para publicar y compartir siempre necesitas Pro o PPU**, incluso en una capacidad. La capacidad es lo que libera a los **lectores** (en F64+).

### Ejemplo
Una empresa de 300 empleados: 10 analistas crean informes y 290 solo los consultan.

- **Opción A**: 300 licencias Pro. Sencillo, sin capacidad.
- **Opción B**: 10 Pro + una capacidad F64 para las áreas de consumo; los 290 lectores pueden usar licencia gratuita. Conviene cuando el costo de la capacidad es menor que las licencias Pro de los lectores, o cuando además necesitas Fabric.
- **Opción C**: 10 PPU para un equipo que necesita modelos grandes, y el resto en Pro; pero ojo, lo que publiquen en áreas PPU solo lo ven usuarios PPU.

Los precios cambian (hubo un aumento en 2025) y dependen del contrato; haz la cuenta con la [página oficial de precios](https://www.microsoft.com/es-mx/power-platform/products/power-bi/pricing) el día que decidas.

### Cuándo usarlo y cuándo no
- **Free**: aprender, practicar con Desktop y uso personal.
- **Pro**: el estándar para equipos pequeños y medianos que colaboran.
- **PPU**: equipos acotados que necesitan funciones avanzadas sin pagar una capacidad completa.
- **Capacidad F64+**: muchos lectores, cargas pesadas o uso de Fabric.
- **Capacidad F pequeña**: pruebas de Fabric, Direct Lake o Embedded; recuerda que los lectores siguen necesitando Pro.

### Errores comunes
- **Creer que Free comparte.** Un usuario Free no puede compartir con su equipo; si se lo "compartes" desde un área compartida (no Premium), el otro tampoco podrá abrirlo.
- **Poner contenido en un área PPU para usuarios Pro.** No lo podrán ver.
- **Memorizar precios o SKUs.** Microsoft cambia nombres, umbrales y precios (el paso de Premium P a Fabric F es el ejemplo más reciente). Para una decisión real, consulta siempre la documentación de licencias y la página de precios del momento.
- **Olvidar la licencia del autor en una capacidad.** La capacidad no elimina la necesidad de Pro para publicar.

### Para profundizar
- [Licencias por usuario y por capacidad del servicio Power BI](https://learn.microsoft.com/es-mx/power-bi/fundamentals/service-features-license-type)
- [Licencias de Microsoft Fabric](https://learn.microsoft.com/es-mx/fabric/enterprise/licenses)
- [Retiro de las SKU P de Power BI Premium](https://learn.microsoft.com/es-mx/power-bi/support/premium-migration-overview)
- [Precios de Power BI](https://www.microsoft.com/es-mx/power-platform/products/power-bi/pricing)

Relacionado: [[Power BI y Microsoft Fabric]], [[Requisitos y licencias para usar Copilot en Power BI]], [[Áreas de trabajo (workspaces)]].

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre una licencia por usuario y una capacidad?
> Por usuario (Free, Pro, Premium por usuario) se asigna a cada persona y define qué puede crear/compartir. Una **capacidad** es un conjunto de recursos dedicados que se compra para la organización y se asigna a workspaces; con capacidad suficiente, usuarios sin Pro pueden consumir contenido. Precios y umbrales: (verificar en la página oficial).

> [!question]- Con licencia Free, ¿puedo compartir un informe con mi equipo?
> En general no: Free sirve para uso personal (Mi área de trabajo). Para compartir y colaborar hace falta Pro/PPU para ambas partes, o que el contenido esté en una capacidad que permita consumo a usuarios Free. (Verificar en la página oficial.)

> [!question]- ¿Por qué no conviene memorizar precios o SKUs de licencias?
> Microsoft los cambia con frecuencia (por ejemplo, la migración de capacidades Premium hacia SKUs de Fabric). Para decisiones reales se consulta siempre la página oficial de precios y la documentación de licencias.

