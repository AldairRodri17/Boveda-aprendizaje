---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Seguridad
tags: [seguridad]
---
# OWASP Top 10

Parte de: [[12-Seguridad/01 Fundamentos de seguridad/_Índice Fundamentos de seguridad|Fundamentos de seguridad]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **OWASP Top 10** es un documento de concientización de la fundación OWASP que agrupa en diez categorías los riesgos de seguridad más relevantes en aplicaciones web, a partir de datos de pruebas reales y de una encuesta a la comunidad. Sirve como **vocabulario común** entre desarrolladores, seguridad y negocio, y como punto de partida para capacitación y revisiones.

Lo que **no** es: no es un estándar completo de verificación, ni una lista de verificación que garantice que una app es segura, ni una lista exhaustiva de vulnerabilidades. Para verificar a fondo existe el **OWASP ASVS** (*Application Security Verification Standard*), y para probar, la guía WSTG.

### Cómo funciona
Cada categoría agrupa varias debilidades (CWE) con una causa común. La edición **2021** es la que más vas a encontrar citada en contratos, cursos y herramientas:

| Código | Categoría 2021 | En tu stack |
|---|---|---|
| A01 | Control de acceso roto | Endpoint sin `[Authorize]` o sin filtrar por tenant, IDOR |
| A02 | Fallas criptográficas | HTTP sin TLS, contraseñas sin hash fuerte |
| A03 | Inyección (incluye XSS) | SQL concatenado, `innerHTML` con datos del API |
| A04 | Diseño inseguro | Flujo de recuperación de contraseña sin límites |
| A05 | Configuración insegura | Swagger y errores detallados en producción |
| A06 | Componentes vulnerables y desactualizados | Paquetes NuGet/npm con CVE |
| A07 | Fallas de identificación y autenticación | Sin MFA, sesiones que no expiran |
| A08 | Fallas de integridad de software y datos | Pipeline que ejecuta paquetes sin verificar |
| A09 | Fallas de registro y monitoreo | No hay logs de logins fallidos |
| A10 | SSRF | La API descarga cualquier URL que le manden |

**Por qué el control de acceso roto encabeza 2021**: subió del quinto lugar porque fue la categoría con más ocurrencias en los datos de las aplicaciones analizadas (OWASP indica que el 94 % de las aplicaciones se probó para alguna forma de esta falla). La razón de fondo es que la autorización **no la resuelve un framework por ti**: depende de reglas de negocio ("este usuario solo ve los pedidos de su tenant") que hay que aplicar en cada endpoint y cada consulta, y que los escáneres automáticos detectan mal porque no conocen esas reglas.

OWASP ya publicó la edición **2025**, que mantiene el control de acceso roto en primer lugar e incorpora categorías como fallas en la **cadena de suministro de software** y el **manejo incorrecto de condiciones excepcionales**. Conoce ambas: muchas organizaciones seguirán citando la 2021 por un tiempo.

### Ejemplo
Usar el Top 10 como checklist de revisión de un PR de Itzamna.Dionisio:

```csharp
[ApiController]
[Route("api/pedidos")]
[Authorize(Policy = "Ventas")] // A01: autorización explícita
public sealed class PedidosController(IObtenerPedido consulta) : ControllerBase
{
    [HttpGet("{id:int}")]
    public async Task<IActionResult> Obtener(int id, CancellationToken ct)
    {
        var tenant = User.FindFirst("tenant")!.Value;   // A01: tenant del token, no de la URL
        var pedido = await consulta.EjecutarAsync(id, tenant, ct); // A03: SQL parametrizado
        return pedido is null ? NotFound() : Ok(pedido);   // sin revelar si existe en otro tenant
    }
}
```

Preguntas rápidas en el PR: ¿quién puede llamar esto? (A01) ¿Hay datos de entrada que lleguen a SQL, HTML o comandos? (A03) ¿Se registra lo importante? (A09) ¿Se agregó algún paquete? (A06).

### Cuándo usarlo y cuándo no
Úsalo para capacitar, priorizar y estructurar revisiones. No lo uses como criterio único de "aprobado": para requisitos verificables usa ASVS, y para tu caso concreto, tu propio [[Mentalidad de seguridad y modelado de amenazas|modelado de amenazas]].

### Errores comunes
- **"Pasamos el Top 10, ya somos seguros".** La respuesta: el Top 10 es una lista de concientización, no de cobertura; cubre categorías amplias y frecuentes, pero no tus reglas de negocio, ni la operación, ni el resto de riesgos. Un escáner que "pasa" tampoco prueba la autorización. Propón medir contra ASVS y modelar amenazas.
- **Memorizar los nombres sin saber prevenirlos.** Lo útil es la sección de prevención de cada categoría.
- **Citar la edición equivocada** en reportes o contratos; aclara siempre el año.
- **Ignorar A09** porque "no es una vulnerabilidad": sin registros no detectas las demás.

### Para profundizar
- [OWASP Top 10 (proyecto)](https://owasp.org/www-project-top-ten/)
- [OWASP Top 10:2025](https://top10.owasp.org/2025/0x00_2025-Introduction/)
- [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es el OWASP Top 10 y qué NO es?
> Es un documento de concientización de OWASP con las 10 categorías de riesgo más relevantes en aplicaciones web, armado con datos de la industria y encuestas. **No** es un estándar de verificación completo: para eso existe OWASP ASVS.

> [!question]- Menciona al menos cinco categorías de la edición 2021.
> Control de acceso roto (A01), fallas criptográficas (A02), inyección (A03, incluye XSS), diseño inseguro (A04), configuración insegura (A05), componentes vulnerables y obsoletos (A06), fallas de identificación y autenticación (A07), fallas de integridad de software y datos (A08), fallas de registro y monitoreo (A09) y SSRF (A10).
>
> OWASP preparaba una edición nueva; revisa cuál es la vigente en owasp.org (Pendiente de confirmar).

> [!question]- Tu equipo dice "pasamos el Top 10, ya somos seguros". ¿Qué le respondes?
> Que el Top 10 es un piso mínimo, no una certificación. Cubre categorías generales; los riesgos de negocio (por ejemplo, lógica de descuentos) o de tu arquitectura no aparecen. Hay que complementarlo con modelado de amenazas y con ASVS.

> [!question]- ¿Por qué "Control de acceso roto" encabeza la lista 2021?
> Porque fue la categoría que más aparecía en las aplicaciones analizadas: endpoints que no verifican si el usuario puede ver o modificar ese recurso (IDOR, falta de filtro por tenant, rutas de administración sin rol).

