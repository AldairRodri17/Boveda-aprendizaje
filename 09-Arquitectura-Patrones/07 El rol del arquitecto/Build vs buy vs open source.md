---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: borrador
area: Arquitectura y patrones
proyectos: ["[[Hecate]]", "[[Pochteca.Nomisma]]"]
tags: [arquitectura-patrones, rol-arquitecto, decisiones]
---
# Build vs buy vs open source

Parte de: [[09-Arquitectura-Patrones/07 El rol del arquitecto/_Índice El rol del arquitecto|El rol del arquitecto]] · [[Temario]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Ante una necesidad (login, pagos, correo, reportes, búsqueda...), el arquitecto decide entre tres caminos:

| Opción | Qué significa | Pagas con... |
|---|---|---|
| **Build** (construir) | Lo programa y mantiene tu equipo | Tiempo del equipo, para siempre |
| **Buy** (comprar) | Usas un producto o servicio de un proveedor (SaaS, API, licencia) | Dinero recurrente y dependencia del proveedor |
| **Open source** (adoptar) | Usas software libre, pero tú lo instalas, actualizas y operas | Operación, actualizaciones y seguridad |

También existe el **híbrido**: compras la base y construyes encima solo lo que te diferencia (por ejemplo, usas Stripe y construyes tu lógica de comisiones de marketplace).

### Cómo decidir
1. **¿Es lo que hace especial al negocio?** Si es el *core domain* (ver [[Subdominios (core, supporting y generic)]]), inclínate a construir. Si es genérico (login, correo, pagos, facturación), inclínate a comprar o adoptar.
2. **Costo total de propiedad** ([[Costo total de propiedad (TCO) de una solución]]): desarrollo + mantenimiento + soporte + seguridad + infraestructura durante 3 a 5 años, no solo el primer mes.
3. **Tiempo para llegar al mercado:** comprar sale en días, construir en meses.
4. **Control y lock-in:** ¿qué pasa si el proveedor sube precios, cambia el API o desaparece? ¿Hay forma de salir? (ver [[Lock-in de proveedor (cuándo importa y cuándo no)]]).
5. **Capacidad del equipo:** ¿alguien sabe hacerlo y mantenerlo dentro de dos años?
6. **Requisitos legales y de datos:** residencia de datos, PCI DSS, privacidad ([[Privacidad y protección de datos (ley mexicana y GDPR)]]).
7. **Licencia** (solo open source): ¿te obliga a liberar tu código? (ver [[Licencias de software open source (MIT, Apache, GPL y AGPL)]]).

### Ejemplo con tus proyectos

**Pagos en [[Pochteca.Nomisma]] → buy.** Se usan [[Stripe]], [[Stripe Connect]] y [[OpenPay]]. Construir una pasarela exige certificación PCI DSS, integración con bancos, OXXO y SPEI, y manejo de fraude. Es genérico para tu negocio y carísimo de mantener: comprar es lo correcto. Lo que sí se construye es la capa propia (comisiones, conciliación, estados internos).

**SSO en [[Hecate]] → build.** Alternativas: Microsoft Entra ID o Auth0 (buy), Keycloak (open source).

| Criterio | Hecate (build) | Entra ID / Auth0 (buy) | Keycloak (open source) |
|---|---|---|---|
| Control del flujo y la UI | Total | Limitado a lo configurable | Alto |
| Costo directo | Bajo | Por usuario activo | Bajo |
| Mantenimiento (llaves, MFA, parches) | **Todo tuyo** | Del proveedor | Tuyo (actualizar y operar) |
| Riesgo de seguridad | Alto si hay un error | Bajo | Medio |
| Tiempo para agregar MFA o login social | Semanas | Configuración | Configuración |

Ninguna columna es "la correcta" en abstracto: depende de cuántos usuarios hay, cuánto pesa el control y quién va a mantenerlo. Lo importante es que la decisión quede escrita.

### Matriz de decisión (plantilla)
Da un peso a cada criterio (1 a 5) y califica cada opción (1 a 5). Multiplica y suma.

| Criterio | Peso | Build | Buy | Open source |
|---|---|---|---|---|
| Diferenciación para el negocio | | | | |
| Costo total a 3 años | | | | |
| Tiempo de entrega | | | | |
| Control y salida del proveedor | | | | |
| Capacidad del equipo | | | | |
| Seguridad y cumplimiento | | | | |
| **Total** | | | | |

El resultado se documenta en un [[ADR - registro de decisiones de arquitectura|ADR]]: contexto, opciones, decisión, consecuencias y **cuándo revisarla** (por ejemplo, "si pasamos de 10,000 usuarios").

### Cuándo construir y cuándo no
- **Construye** si es tu ventaja competitiva, si nada en el mercado cubre el caso o si el costo de comprar a tu escala supera el de mantenerlo.
- **Compra** si es genérico, regulado (pagos, identidad) o urgente.
- **Adopta open source** si quieres control sin escribirlo desde cero y tienes quién lo opere.

### Errores comunes
1. **Síndrome de "no inventado aquí":** construir porque "nosotros lo hacemos mejor" sin calcular el mantenimiento.
2. **Comparar solo el precio de la licencia** contra el sueldo de un mes de desarrollo, en lugar del costo a varios años.
3. **Olvidar el costo de salida:** migrar datos y usuarios fuera de un proveedor puede costar más que la compra inicial.
4. **Open source "gratis":** no se cuenta el tiempo de actualizar, parchar vulnerabilidades y operarlo.
5. **No documentar la decisión:** en un año nadie recuerda por qué se eligió así y se vuelve a discutir.

### Para profundizar
- *Software Architecture: The Hard Parts* (Ford, Richards, Sadalage y Dehghani): decisiones con trade-offs.
- *Learning Domain-Driven Design* (Vlad Khononov): subdominios core, supporting y generic, y su relación con construir o comprar.
- [[Libro - The Software Architect Elevator]]: el arquitecto como puente con negocio.

## Laboratorio

**Lab 1 - Decisión documentada para Hecate**
1. Llena la matriz de decisión con tres opciones para el SSO de Hecate: seguir construyendo, Entra ID y Keycloak.
2. Asigna pesos según la realidad del proyecto (usuarios, equipo, presupuesto).
3. Escribe un ADR con la decisión y la condición para revisarla.
4. Explícale la decisión en 3 minutos a alguien no técnico (o a Claude en una sesión de estudio).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuáles son las tres opciones y qué "pagas" en cada una?
> Build: tiempo del equipo para siempre. Buy: dinero recurrente y dependencia del proveedor. Open source: operación, actualizaciones y seguridad.

> [!question]- ¿Qué pregunta pesa más para decidir?
> Si es lo que diferencia al negocio (core domain). Lo diferenciador se construye; lo genérico se compra o se adopta.

> [!question]- ¿Por qué no basta comparar el precio de la licencia contra el costo de desarrollarlo?
> Porque construir incluye mantener, corregir, dar soporte y seguridad durante años (TCO), y comprar incluye el costo de salida del proveedor.

> [!question]- ¿Por qué comprar pagos en Nomisma y por qué es discutible construir el SSO en Hecate?
> Pagos es genérico, regulado (PCI DSS) y caro de mantener. Un SSO también es genérico y sensible en seguridad: construirlo da control, pero obliga a mantener llaves, MFA y parches; Entra ID o Keycloak reducen ese riesgo.

> [!question]- ¿Qué debe incluir el ADR de una decisión build vs buy?
> Contexto, opciones evaluadas, criterios y pesos, decisión, consecuencias y la condición que haría revisar la decisión.
