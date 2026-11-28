---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Automatización, DevOps y operación
tags: [automatizacion-devops]
contenido: completo
---
# Tasa de fallos de cambios (DORA)

Parte de: [[Métricas DORA]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **tasa de fallos de cambios** (*change failure rate*) es el **porcentaje de despliegues a producción que provocan una falla** que requiere intervención inmediata: un rollback, un hotfix, un *roll forward* urgente, desactivar un flag o atender un incidente. Es una de las [[Métricas DORA]] del grupo de inestabilidad.

```text
tasa de fallos = despliegues que causaron falla / despliegues totales
```

De 20 despliegues, si 3 necesitaron rollback o hotfix, la tasa es 3 / 20 = 15 %.

### Cómo funciona
**Qué necesitas registrar para calcularla**:
- **Todos los despliegues a producción** del servicio (fecha, versión), por ejemplo del historial del environment en [[Azure Pipelines]].
- **Qué despliegues causaron una falla**: un vínculo entre el incidente (o el hotfix, o el rollback) y el despliegue que lo originó. Esto es lo difícil: requiere que, al registrar un incidente, alguien anote qué cambio lo causó. En Azure DevOps puedes crear un work item de tipo *Incidente* con un campo «Despliegue causante» o etiquetar los runs de rollback.
- Una **definición acordada** de «falla»: ¿un bug menor reportado una semana después cuenta? Lo habitual es contar solo fallas que degradan el servicio y requieren remediación inmediata, no cualquier bug.

DORA introdujo además la **tasa de retrabajo** (despliegues no planeados para corregir problemas en producción entre despliegues totales), que se calcula sin depender tanto de la clasificación manual de incidentes.

**Cómo se baja sin desplegar menos**: desplegar menos seguido **no** la reduce de forma sostenible; los lotes grandes fallan más. Lo que funciona:
- **Lotes pequeños**: menos cambios por despliegue, menos que pueda fallar y más fácil de diagnosticar.
- **Pruebas automatizadas** en el pipeline: unitarias, de integración ([[Pruebas de integración con WebApplicationFactory]], [[Testcontainers]]) y de humo después de desplegar.
- **Ambientes parecidos a producción** (misma configuración, IaC) para que lo probado se comporte igual.
- **Migraciones compatibles** (expand/contract) para que la base no rompa el despliegue.
- **Exposición gradual**: [[Despliegue canary]] y [[Feature flags]] limitan el impacto y permiten detectar antes de que el fallo cuente para todos.
- **Revisión de código** enfocada y análisis estático ([[Análisis estático (SonarQube y linters)]]).
- **Aprender de cada fallo**: [[Postmortems sin culpables]] con acciones que sí se cierran.

### Ejemplo
Consulta sobre una tabla propia donde el pipeline registra despliegues y el equipo marca los incidentes (en el estilo del usuario, en un archivo `.sql`):

```sql
-- Sql/Metricas/TasaFallosCambios.sql
SELECT
    COUNT(*)                                                 AS Despliegues,
    SUM(CASE WHEN d.CausoFalla = 1 THEN 1 ELSE 0 END)        AS Fallidos,
    CAST(100.0 * SUM(CASE WHEN d.CausoFalla = 1 THEN 1 ELSE 0 END)
         / NULLIF(COUNT(*), 0) AS DECIMAL(5, 2))             AS TasaFallosPct
FROM dbo.Despliegue AS d
WHERE d.Servicio = @Servicio
  AND d.Ambiente = 'prod'
  AND d.FechaUtc >= DATEADD(DAY, -30, SYSUTCDATETIME());
```

`dbo.Despliegue.CausoFalla` se actualiza desde el work item del incidente o cuando un pipeline de rollback se ejecuta indicando a qué despliegue revierte.

### Cuándo usarlo y cuándo no
- **Úsala** junto con la [[Frecuencia de despliegue (DORA)]]: juntas dicen si vas rápido de forma segura.
- **No la uses para castigar**: si cada fallo registrado trae culpables, el equipo dejará de registrarlos y la métrica mentirá.

### Errores comunes
1. **Subregistro**: solo se cuentan los incidentes grandes, y los hotfix silenciosos desaparecen.
2. **Definición cambiante** de lo que es falla.
3. **Reducirla desplegando menos**, lo que empeora el resto de las métricas.
4. **Contar bugs que no vienen de un despliegue** (por ejemplo, una caída del proveedor): esos van a la confiabilidad general, no a esta métrica.

### Para profundizar
- [DORA: métricas de entrega de software](https://dora.dev/guides/dora-metrics-four-keys/)
- [DORA: capacidad de automatización de pruebas](https://dora.dev/capabilities/test-automation/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué mide?
> El porcentaje de despliegues a producción que causan una falla que requiere atención inmediata (rollback, hotfix, parche).

> [!question]- ¿Cómo se baja sin desplegar menos?
> Mejores pruebas automáticas, revisiones, canary y feature flags que limitan el impacto y permiten detectar antes.

> [!question]- ¿Qué necesitas registrar para calcularla?
> Vincular cada incidente o rollback al despliegue que lo causó (p. ej. con el número de build en el work item del incidente).

