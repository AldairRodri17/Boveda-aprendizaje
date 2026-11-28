---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: borrador
area: Filosofía práctica
tags: [filosofia, etica, ia]
contenido: completo
---
# Ética en la tecnología y la IA

Parte de: [[66-Filosofia-Practica/04 Ética/_Índice Ética|Ética]] · Relacionado: [[21-IA-Machine-Learning/_Índice IA y Machine Learning|IA y Machine Learning]] · [[12-Seguridad/_Índice Seguridad|Seguridad]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** → **visto** → **dominado**.

## Contenido

### Qué es
La aplicación de la ética a las decisiones que tomamos al **diseñar, construir y operar software**. Como desarrollador, tus decisiones técnicas afectan a personas que nunca conocerás: qué datos se guardan, quién puede ver qué, cómo decide un algoritmo.

### Cómo funciona: temas principales
| Tema | Pregunta ética | Ejemplo |
|---|---|---|
| **Privacidad** | ¿Qué datos necesito realmente? | Guardar la ubicación de usuarios "por si acaso" |
| **Seguridad** | ¿Protejo a quien confía en mí? | Contraseñas en texto plano |
| **Sesgo y justicia** | ¿El sistema trata igual a todos? | Un modelo de crédito que penaliza ciertos códigos postales |
| **Transparencia** | ¿Puede la persona entender y apelar una decisión? | Rechazo automático sin explicación |
| **Diseño manipulador** | ¿Respeto la autonomía del usuario? | "Dark patterns": cancelar una suscripción en 12 pasos |
| **Responsabilidad** | ¿Quién responde si falla? | Un sistema de IA que da un consejo médico equivocado |
| **Impacto laboral y social** | ¿A quién beneficia y a quién perjudica? | Automatizar tareas sin plan para las personas afectadas |

**IA en particular.** Los modelos aprenden de datos históricos, así que pueden **reproducir sesgos**. Además pueden "alucinar" (inventar información con seguridad). Principios comunes en marcos internacionales (OCDE, UNESCO, la Ley de IA de la Unión Europea): supervisión humana, transparencia, robustez, privacidad, no discriminación y rendición de cuentas.

### Ejemplo práctico
Te piden un endpoint que exporte todos los datos de clientes para "análisis de marketing".
- **Lentes éticos** ([[Corrientes de la ética (consecuencias, deber y virtud)]]): ¿qué daño causa una fuga? ¿el cliente aceptó ese uso? ¿qué haría un ingeniero íntegro?
- **Acción:** proponer solo los campos necesarios, anonimizar, registrar accesos y verificar el aviso de privacidad (en México, la ley de protección de datos personales).

### Preguntas para tus revisiones de diseño
1. ¿Qué datos personales toca esto y por qué?
2. ¿Quién podría salir perjudicado si falla o se usa mal?
3. ¿Podemos explicar la decisión al usuario?
4. ¿Hay una persona que pueda revisar y corregir?

### Errores comunes
- "Yo solo escribo código, la decisión es del negocio": tu conocimiento técnico te da responsabilidad para advertir.
- Pensar en ética solo al final; es más barato incluirla en el diseño.

### Para profundizar
- [[Aplicar la filosofía en el trabajo]]
- [[12-Seguridad/_Índice Seguridad|Seguridad]]

## Autoevaluación

> [!question]- ¿Por qué un modelo de IA puede ser injusto aunque nadie lo programe así?
> Porque aprende de datos históricos que pueden contener sesgos y los reproduce.

> [!question]- ¿Qué es un dark pattern?
> Un diseño de interfaz que manipula al usuario para que haga algo que no haría libremente.

> [!question]- Menciona tres principios comunes de los marcos de IA responsable.
> Supervisión humana, transparencia y no discriminación (también privacidad, robustez y rendición de cuentas).

## Práctica

- Agrega las cuatro preguntas de diseño a tu plantilla de ARQUITECTURA o de pull request y úsalas en tu próximo proyecto.
