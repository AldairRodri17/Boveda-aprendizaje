---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Certificaciones
tags: [certificaciones]
contenido: completo
---
# Renovación anual de certificaciones de Microsoft

Parte de: [[18-Certificaciones/01 Azure ruta hasta arquitecto/_Índice Azure ruta hasta arquitecto|Azure ruta hasta arquitecto]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es

Las certificaciones **associate, expert y specialty** de Microsoft vencen **un año** después de obtenerlas. Para mantenerlas no vuelves a presentar el examen: haces una **evaluación de renovación** gratuita y en línea en Microsoft Learn. Las **Fundamentals no vencen**, así que no se renuevan. Existe porque Azure cambia rápido; la renovación comprueba que sigues al día con los cambios del último año.

### Cómo funciona

```mermaid
sequenceDiagram
  participant T as Tú
  participant L as Microsoft Learn
  T->>L: Obtienes la certificación (día 0)
  Note over L: Vigencia de 1 año
  L-->>T: A los 6 meses se abre la ventana de renovación (aviso por correo)
  T->>L: Presentas la evaluación (en línea, sin supervisor)
  alt Apruebas
    L-->>T: Se extiende 1 año desde la fecha de vencimiento original
  else Repruebas
    T->>L: Reintentas (inmediato la 1.ª vez; luego 24 h entre intentos)
  end
```

Puntos clave según las preguntas frecuentes oficiales y la política de reintentos:

- **Costo:** gratis.
- **Ventana:** se abre **6 meses antes** de la fecha de vencimiento.
- **Formato:** evaluación en línea en tu perfil de Learn, sin Pearson VUE ni supervisor; se enfoca en lo que cambió del rol (número de preguntas y duración: Pendiente de confirmar, varía por certificación).
- **Reintentos:** después del primer fallo puedes reintentar de inmediato; a partir de ahí, 24 horas entre intentos. No hay límite de intentos, pero tienes que aprobar **antes** de la fecha de vencimiento.
- **Si se vence:** pierdes la certificación y tendrías que volver a presentar el examen completo (con costo).

### Exámenes retirados

Si Microsoft anuncia el retiro del examen de una certificación que tienes, debes renovar **antes** de la fecha de retiro. Después ya no se puede renovar y, al vencer, pasa a la sección de certificaciones históricas de tu perfil. Es justo lo que vivieron quienes tenían Azure Developer Associate (AZ-204, retirado el 31 de julio de 2026) o Azure Security Engineer Associate (AZ-500, retirado el 31 de agosto de 2026). El detalle exacto de cada caso conviene verlo en tu perfil de Learn (Pendiente de confirmar caso por caso).

### Ejemplo: tu rutina de renovación

Supón que apruebas AZ-104 el 15 de marzo de 2027.

1. En la nota [[AZ-104 Azure Administrator Associate]] llenas `fecha_obtenida: 2027-03-15` y `vence: 2028-03-15`.
2. Pones un recordatorio en tu calendario para el **15 de septiembre de 2027** (apertura de la ventana).
3. En octubre repasas las novedades de Azure de ese año (redes, identidad, Bicep) con la colección de renovación que te sugiere Learn.
4. Presentas la evaluación una tarde. Si fallas, el informe te dice qué áreas reforzar; repasas y reintentas al día siguiente.
5. Actualizas `vence` en la nota.

Si tienes varias certificaciones, junta sus fechas en [[Plan de certificaciones (calendario y presupuesto)]] para no tener tres renovaciones el mismo mes.

### Cuándo usarlo y cuándo no

- **Renueva** las que respaldan tu rol actual o tu siguiente paso (AZ-104 si vas a AZ-305).
- **Puedes dejar vencer** una certificación que ya no usas; no se borra de tu historial, solo aparece como vencida o histórica.
- No esperes al último mes: si hay un problema técnico o fallas varias veces, te quedas sin margen.

### Errores comunes

1. **Usar la cuenta del trabajo.** Si dejas la empresa pierdes acceso al perfil y a las renovaciones. Usa una cuenta personal de Microsoft y, si ya tienes certificaciones en otra, pide la fusión de perfiles a soporte.
2. **No leer los correos de Microsoft Learn.** El aviso de apertura llega por correo; revisa que no caiga en spam.
3. **Pensar que las Fundamentals vencen** y "renovarlas" volviendo a presentar el examen. No hace falta.
4. **Dejar pasar la fecha de retiro** de un examen creyendo que todavía tienes todo el año.

### Para profundizar

- [Renovación de certificaciones: preguntas frecuentes](https://learn.microsoft.com/en-us/credentials/certifications/renew-your-microsoft-certification-faq)
- [Política de reintentos (incluye la de renovación)](https://learn.microsoft.com/en-us/credentials/support/retake-policy)
- [Exámenes retirados](https://learn.microsoft.com/en-us/credentials/support/retired-certification-exams)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuándo y cuánto cuesta renovar?
> Las associate, expert y specialty vencen al año. La evaluación de renovación es **gratis**, en línea, y se abre **6 meses antes** del vencimiento.

> [!question]- Reprobaste la evaluación de renovación. ¿Qué haces?
> Puedes reintentar de inmediato; después del segundo intento debes esperar al menos 24 h entre intentos. No hay límite, pero debe ser antes de la fecha de vencimiento.

> [!question]- ¿Las Fundamentals se renuevan?
> No: no vencen.

> [!question]- ¿Qué pasa si el examen se retira antes de tu vencimiento?
> Microsoft indica renovar antes de la fecha de retiro. Después, la certificación deja de poder renovarse y pasa a históricas al vencer.

