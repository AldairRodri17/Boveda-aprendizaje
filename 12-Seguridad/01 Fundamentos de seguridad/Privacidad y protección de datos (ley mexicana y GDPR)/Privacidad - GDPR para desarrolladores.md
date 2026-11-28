---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Seguridad
tags: [seguridad]
contenido: completo
---
# Privacidad - GDPR para desarrolladores

Parte de: [[Privacidad y protección de datos (ley mexicana y GDPR)]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **GDPR** (Reglamento General de Protección de Datos, Reglamento (UE) 2016/679) es la ley de la Unión Europea sobre datos personales. Aplica a quien trata datos de personas que están en la UE, aunque la empresa esté fuera, cuando les ofrece bienes o servicios o monitorea su comportamiento. Es la referencia mundial en privacidad y muchas leyes la imitan.

Para un desarrollador, el GDPR se traduce en requisitos técnicos concretos: saber qué datos guardas y por qué, poder exportarlos y borrarlos, protegerlos y detectar y reportar brechas.

### Cómo funciona
**Bases legales (artículo 6).** Todo tratamiento necesita al menos una:
1. **Consentimiento** libre, específico, informado e inequívoco (y revocable con la misma facilidad).
2. **Contrato**: necesario para cumplir un contrato con la persona (procesar su pedido).
3. **Obligación legal** (conservar facturas).
4. **Intereses vitales** (emergencias médicas).
5. **Misión de interés público** o ejercicio de poderes públicos.
6. **Interés legítimo**, siempre que no prevalezcan los derechos de la persona (prevención de fraude, por ejemplo).

**Derechos de los titulares**: acceso, rectificación, supresión ("derecho al olvido", artículo 17), limitación, portabilidad y oposición, entre otros.

**El derecho al olvido** implica, en tu sistema, poder **localizar todos los datos de una persona y borrarlos o anonimizarlos** en un plazo razonable: tablas principales, tablas relacionadas, archivos subidos, índices de búsqueda, cachés, logs, copias en sistemas de terceros (CRM, correo, analítica) y, con una política clara, respaldos (normalmente no se editan; se documenta que el dato expira con el respaldo y que no se restaurará). Tiene excepciones, como datos que debes conservar por obligación legal.

**Brechas (artículo 33)**: el responsable debe notificar a la autoridad de control **sin dilación indebida y, cuando sea posible, a más tardar 72 horas** después de tener conocimiento, salvo que sea improbable que la brecha suponga un riesgo para las personas. Si el riesgo es alto, también hay que avisar a los afectados.

**Privacidad desde el diseño y por defecto (artículo 25)**: minimizar datos, seudonimizar, configurar lo más privado como opción predeterminada.

### Ejemplo
Diseño de un endpoint de supresión en Itzamna.Dionisio:

```csharp
[HttpDelete("api/clientes/{id:int}/datos-personales")]
[Authorize(Policy = "Privacidad")]
public async Task<IActionResult> Suprimir(int id, CancellationToken ct)
{
    // 1. Anonimiza en la base (procedimiento con todas las tablas relacionadas)
    await suprimir.EjecutarAsync(id, ct);       // dbo.Clientes_Anonimizar @ClienteId
    // 2. Borra archivos del cliente en Blob Storage
    await archivos.BorrarCarpetaAsync($"clientes/{id}", ct);
    // 3. Encola la supresión en sistemas externos (CRM, correo)
    await cola.EnviarAsync(new SuprimirEnExternos(id), ct);
    // 4. Registra el hecho (sin los datos borrados) como evidencia de cumplimiento
    logger.LogInformation("Supresión de datos del cliente {ClienteId} solicitada por {Usuario}", id, User.Identity!.Name);
    return NoContent();
}
```

Anonimizar en lugar de borrar físicamente conserva la integridad de pedidos y facturas, que pueden tener obligación legal de conservarse.

### Cuándo usarlo y cuándo no
Aplica si tienes usuarios en la UE o trabajas para una empresa que los tiene, aunque tu equipo esté en México. Aunque no aplique, sus prácticas (inventario de datos, minimización, supresión) son buena ingeniería y te ayudan con la ley mexicana. Ver [[Privacidad - ley mexicana de datos personales (LFPDPPP)]].

### Errores comunes
- **Pedir consentimiento para todo**, incluso cuando la base correcta es el contrato; si luego lo retiran, te quedas sin base.
- **Borrar solo la tabla principal** y dejar copias en logs, analítica y archivos.
- **No tener un inventario de datos**: no puedes borrar lo que no sabes dónde está.
- **Registrar datos personales en logs sin necesidad**. Ver [[Logs y datos sensibles]].
- **Darle 72 horas a la investigación técnica** antes de avisar al área legal: el plazo corre desde que la organización tiene conocimiento.

### Para profundizar
- [Texto oficial del Reglamento (UE) 2016/679 en EUR-Lex](https://eur-lex.europa.eu/eli/reg/2016/679/oj)
- [Comité Europeo de Protección de Datos (EDPB)](https://www.edpb.europa.eu/)
- [OWASP User Privacy Protection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/User_Privacy_Protection_Cheat_Sheet.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué bases legales contempla el GDPR para tratar datos?
> Consentimiento, contrato, obligación legal, intereses vitales, interés público e interés legítimo.

> [!question]- ¿Qué implica el "derecho al olvido" para tu sistema?
> Que debes poder localizar y borrar (o anonimizar) los datos de una persona en todas las tablas, respaldos según política, logs y servicios de terceros. Diséñalo desde el inicio.

> [!question]- ¿Cuánto tiempo hay para notificar una brecha a la autoridad bajo GDPR?
> Sin demora indebida y, cuando sea posible, en un máximo de 72 horas desde que se tuvo conocimiento, salvo que sea improbable que implique riesgo para las personas.

