---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
contenido: completo
---
# Recuperación ante desastres (RPO y RTO)

Parte de: [[08-Automatizacion-DevOps/02 Operación en producción/_Índice Operación en producción|Operación en producción]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **recuperación ante desastres (DR)** es el conjunto de estrategias y procedimientos para **restaurar un sistema después de un evento grave**: corrupción o borrado de datos, ransomware, caída de una región, un despliegue que destruyó información. Se diseña alrededor de dos números que acuerda el negocio:
- **RPO** (*Recovery Point Objective*): **cuántos datos puedes perder**, medido en tiempo. «RPO de 15 min» = como máximo se pierden los últimos 15 minutos de transacciones.
- **RTO** (*Recovery Time Objective*): **cuánto puede tardar** en volver a funcionar el servicio desde que ocurre el desastre. «RTO de 1 h».

### Cómo funciona
El RPO lo determina **qué tan seguido copias los datos** (respaldos, replicación); el RTO lo determina **cuánto tardas en tener todo operando** (restaurar, validar, redirigir tráfico, avisar).

```mermaid
flowchart LR
    U[Último punto recuperable] -->|RPO: datos perdidos| X[Desastre]
    X -->|RTO: tiempo sin servicio| R[Servicio restaurado]
```

**Ejemplos con respaldos**:
- Respaldo completo diario a las 00:00 y de **log de transacciones cada 15 min**. La base se corrompe a las 10:07. El último log fue a las 10:00, así que puedes restaurar hasta las 10:00 y pierdes **unos 7 minutos**. El **RPO del diseño es 15 min** (el peor caso es que falle justo antes del siguiente respaldo de log).
- Solo respaldo completo diario a las 00:00, y falla a las 23:50: pierdes **casi 24 horas** (23 h 50 min). El RPO de ese diseño es de 24 h.
- Restaurar tarda 40 min y validar 20 min: **60 min** en total. Si el RTO acordado es 30 min, **no cumples**. Opciones: tener una réplica ya sincronizada a la que solo conmutas (geo-replicación o grupo de failover), bases más pequeñas o restauraciones paralelas, automatizar la validación con scripts y pruebas de humo, o renegociar el RTO con el negocio si el costo no se justifica.

**Estrategias, de menor a mayor costo y de mayor a menor RTO**:
1. **Respaldo y restauración**: solo hay copias; ante un desastre recreas la infraestructura (idealmente con [[Infraestructura como código - conceptos|IaC]]) y restauras. Barato; RTO de horas.
2. **Piloto encendido** (*pilot light*): en la región secundaria están los datos replicados y lo mínimo encendido; el cómputo se crea o escala al conmutar. RTO de decenas de minutos a horas.
3. **Warm standby**: una versión reducida pero funcional de todo el sistema corre en la secundaria; al conmutar solo escalas. RTO de minutos.
4. **Activo-activo**: ambas regiones atienden tráfico; perder una casi no se nota. RTO cercano a cero, costo más alto y mayor complejidad. Ver [[Multi-región y alta disponibilidad]].

**Lo que ofrece Azure SQL Database para DR**:
- **Respaldos automáticos** con **restauración a un punto en el tiempo (PITR)** dentro del periodo de retención configurable (por defecto 7 días, ampliable hasta 35), y **retención a largo plazo (LTR)** para años.
- **Geo-restauración**: con respaldos geo-redundantes, restaurar en otra región si la tuya no está disponible (RPO y RTO de horas).
- **Geo-replicación activa**: réplicas secundarias legibles en otra región con replicación asíncrona (RPO de segundos).
- **Grupos de failover**: conmutación de una o varias bases con un *listener* que no cambia de nombre; la conmutación puede ser automática o manual.
- **Redundancia de zona** para fallas dentro de la región.
Revisa en la documentación vigente los RPO/RTO que Microsoft indica para cada opción. Ver [[Backup y restore]] y [[Azure SQL Database]].

### Ejemplo
Plan de DR para [[Itzamna.Dionisio]] con RPO 15 min y RTO 1 h:
- Azure SQL con grupo de failover hacia una segunda región (RPO de segundos).
- PITR de 14 días para el caso «alguien borró datos» (la réplica también replicaría el borrado; la réplica **no** sustituye al respaldo).
- Infraestructura en Bicep para recrear App Service en la región secundaria en minutos.
- [[Runbooks|Runbook]] de conmutación con pasos, responsables y comandos.

```bash
# Simulacro: conmutar el grupo de failover a la secundaria
az sql failover-group set-primary -g rg-dionisio-dr -s sql-dionisio-sec -n fog-dionisio
# Restaurar una copia a un punto en el tiempo para revisar datos borrados (no sobrescribe la original)
az sql db restore -g rg-dionisio-prod -s sql-dionisio -n Dionisio \
  --dest-name Dionisio-restaurada --time "2026-11-09T16:00:00Z"
```

### Cuándo usarlo y cuándo no
- **Todo sistema en producción** necesita al menos respaldos probados y un RPO/RTO conocido.
- Las estrategias caras solo cuando el costo del tiempo fuera y de los datos perdidos lo justifica; el RPO y RTO **los decide el negocio** con los costos sobre la mesa.

### Errores comunes
1. **«Tenemos DR» sin haberlo probado.** Un respaldo que nunca se restauró es una suposición: puede estar corrupto, incompleto, sin permisos o tardar el triple de lo esperado. Programa simulacros periódicos, mide el RTO real y actualiza el runbook.
2. **Confundir replicación con respaldo**: la réplica copia también los errores y los borrados.
3. **Olvidar dependencias**: secretos, DNS, certificados, archivos de Blob, configuración.
4. **Respaldos en la misma suscripción y región** que lo que protegen (y con los mismos permisos que un atacante podría usar).

### Para profundizar
- [Continuidad de negocio en Azure SQL Database](https://learn.microsoft.com/azure/azure-sql/database/business-continuity-high-availability-disaster-recover-hadr-overview)
- [Recuperación con respaldos en Azure SQL](https://learn.microsoft.com/azure/azure-sql/database/recovery-using-backups)
- [Azure Well-Architected: recuperación ante desastres](https://learn.microsoft.com/azure/well-architected/reliability/disaster-recovery)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Respaldo completo diario a las 00:00 y de log cada 15 min. La base se corrompe a las 10:07. ¿Pérdida máxima esperada y RPO del diseño?
> Restauras hasta el último log (10:00): pierdes ≈ **7 min**. El RPO del diseño es **15 min** (peor caso).

> [!question]- Solo hay respaldo completo diario a las 00:00. Falla a las 23:50. ¿Cuántos datos pierdes?
> Casi **24 h** (23 h 50 min). Ese diseño tiene RPO de 24 h.

> [!question]- Restaurar tarda 40 min y validar 20 min. El RTO acordado es 30 min. ¿Cumples? ¿Qué cambiarías?
> No: 60 min > 30 min. Opciones: réplica secundaria lista (geo-replicación / failover group), automatizar la validación o *warm standby*.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Define RPO y RTO.
> **RPO** (Recovery Point Objective): cuántos datos puedes perder, medido en tiempo hacia atrás. **RTO** (Recovery Time Objective): cuánto puedes tardar en volver a operar.

> [!question]- Ordena por costo y RTO: respaldo y restauración, piloto encendido, *warm standby*, activo-activo.
> Del más barato y lento al más caro y rápido: respaldo y restauración (horas) → piloto encendido (lo mínimo corriendo) → *warm standby* (copia reducida lista para escalar) → activo-activo (casi cero).

> [!question]- ¿Qué te ofrece Azure SQL Database para DR?
> Respaldos automáticos con restauración a un punto en el tiempo (PITR), geo-restauración desde respaldos replicados, **geo-replicación activa** y **failover groups** con un listener que no cambia al conmutar.

> [!question]- Error común: «tenemos DR» sin haberlo probado.
> Hasta que no lo ensayas no conoces tu RTO real; suelen faltar DNS, secretos, permisos o pasos manuales. Haz simulacros periódicos.

