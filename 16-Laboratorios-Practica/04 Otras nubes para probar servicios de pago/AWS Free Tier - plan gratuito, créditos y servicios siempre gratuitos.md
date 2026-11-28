---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Laboratorios y práctica
tags: [laboratorios-practica]
contenido: completo
---
# AWS Free Tier - plan gratuito, créditos y servicios siempre gratuitos

Parte de: [[16-Laboratorios-Practica/04 Otras nubes para probar servicios de pago/_Índice Otras nubes para probar servicios de pago|Otras nubes para probar servicios de pago]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El AWS Free Tier es el conjunto de beneficios con los que Amazon Web Services permite a una cuenta nueva probar sus servicios sin pagar al inicio. AWS **renovó el esquema el 15 de julio de 2025**: las cuentas creadas desde esa fecha entran al nuevo modelo de "plan gratuito" con créditos; las anteriores siguen con el Free Tier clásico (de 12 meses). Entender cómo funciona evita el clásico susto del recibo a fin de mes.

### Qué tipos de beneficios gratuitos ofrece AWS
```mermaid
flowchart TB
  C[Créditos para cuenta nueva] --> U[Gastar en servicios elegibles]
  P[Plan gratuito por tiempo limitado] --> S[Select de servicios, sin cargos]
  AF[Servicios "siempre gratuitos"] --> L[Límites mensuales permanentes]
```

En el **nuevo esquema (desde jul-2025)**:
- **Créditos**: hasta **200 USD** para cuentas nuevas. 100 USD al activarse y otros 100 USD que se ganan completando actividades (lanzar y terminar una instancia EC2, configurar una base RDS, crear una función Lambda con URL, probar el playground de Bedrock y **crear un presupuesto con alertas**).
- **Plan gratuito**: dura hasta **6 meses** o hasta que se agoten los créditos, lo que ocurra primero. No cobra salvo que pases a plan de pago.
- **Servicios "siempre gratuitos"**: AWS menciona más de 30 servicios con capa gratuita permanente dentro de límites mensuales (por ejemplo, tradicionalmente cierta cantidad de peticiones de Lambda y almacenamiento de DynamoDB; verificar los límites exactos en la página oficial).

### Qué cuidado es el más importante
**Configurar presupuestos y alertas el primer día** (ver [[Cómo evitar cobros inesperados en la nube (presupuestos y alertas)]]) y revisar **en qué región** creas recursos: lo que no está cubierto por el free tier o por los créditos se cobra. En el nuevo plan gratuito, además, AWS avisa 15, 7 y 2 días antes de que termine el periodo de 6 meses.

### Cómo empezar paso a paso
1. Crea la cuenta y elige el **plan gratuito** (no el de pago) si solo quieres practicar.
2. Antes de crear nada, ve a **AWS Budgets** y define un presupuesto con alerta por correo (una de las actividades que, además, da crédito).
3. Crea recursos pequeños en una sola región; etiqueta lo que levantes.
4. **Apaga o borra** instancias, bases y volúmenes al terminar cada práctica.
5. Revisa el panel de facturación con frecuencia.

### Qué recursos suelen generar cobros sorpresa
Los clásicos: **NAT Gateways**, **IPs públicas** (Elastic IP) sin usar, **instancias o bases de datos olvidadas encendidas**, **volúmenes y snapshots huérfanos** (EBS que quedan tras borrar una VM) y **transferencia de datos** (egress) alta.

### Cuándo usarlo y cuándo no
Úsalo para aprender servicios que tu nube principal no tiene o para preparar certificaciones de AWS. Para tu trabajo diario, tu nube es Azure (ver [[Microsoft Learn - rutas gratuitas de Azure]]). Si solo quieres un servidor Linux siempre encendido sin reloj de cuenta regresiva, [[Oracle Cloud - Always Free]] puede convenir más. Para comparar, revisa [[Comparativa de capas gratuitas de nubes (AWS, Azure, Google Cloud y Oracle)]].

### Errores comunes
- **No poner presupuesto el primer día.** Es la causa número uno de recibos sorpresa.
- **Dejar NAT Gateway o Elastic IP activos.** Cuestan por hora aunque no los uses.
- **Crear en varias regiones y olvidar recursos.** Difíciles de encontrar y de cerrar.
- **Suponer que "free tier" cubre todo.** Solo cubre servicios y límites específicos.

### Para profundizar
- [AWS Free Tier](https://aws.amazon.com/free/)
- [Anuncio del nuevo Free Tier (jul-2025)](https://aws.amazon.com/blogs/aws/aws-free-tier-update-new-customers-can-get-started-and-explore-aws-with-up-to-200-in-credits/)
- [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué tipos de beneficios gratuitos ofrece AWS?
> Créditos para cuentas nuevas, un plan gratuito por tiempo limitado y servicios con capa "siempre gratuita" (por ejemplo cierta cantidad mensual de Lambda o DynamoDB). Las condiciones cambiaron en 2025 (verificar en la página oficial).

> [!question]- ¿Qué cuidado es el más importante?
> Configurar presupuestos y alertas el primer día (ver [[Cómo evitar cobros inesperados en la nube (presupuestos y alertas)]]) y revisar en qué región creas recursos: lo que no está cubierto se cobra.

> [!question]- ¿Qué recursos suelen generar cobros sorpresa?
> NAT Gateways, IPs públicas, instancias o bases de datos olvidadas encendidas, volúmenes y snapshots huérfanos y transferencia de datos.

