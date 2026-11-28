---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Carrera y liderazgo técnico
tags: [carrera-liderazgo]
contenido: completo
---
# Vocabulario técnico en inglés

Parte de: [[14-Carrera-Liderazgo/05 Inglés técnico/_Índice Inglés técnico|Inglés técnico]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
El **vocabulario técnico en inglés** es el conjunto de palabras y expresiones que aparecen en documentación, reuniones y código: términos de despliegue, rendimiento, arquitectura, calidad. Muchos ya los usas en *spanglish* ("hacer el deploy", "mergear"), pero conviene dominarlos en inglés correcto, con su pronunciación común y sin caer en falsos amigos.

### Cómo funciona
Aprende **frases completas**, no palabras sueltas: "*roll back the deployment*" se recuerda mejor que "*rollback* = revertir".

**Despliegue y operación:**

| Inglés | Español | Ejemplo |
|---|---|---|
| *rollout* | despliegue gradual | *We'll do a gradual rollout to 10% of users.* |
| *rollback* / *to roll back* | revertir a la versión anterior | *If it fails, we roll back.* |
| *outage* | caída del servicio | *We had a 20-minute outage.* |
| *on-call* | guardia | *I'm on call this week.* |
| *postmortem* | análisis posterior al incidente | |

**Rendimiento y arquitectura:**

| Inglés | Español |
|---|---|
| *throughput* | rendimiento, caudal (trabajo por unidad de tiempo) |
| *latency* | latencia (tiempo de respuesta) |
| *bottleneck* | cuello de botella |
| *eventually consistent* | con consistencia eventual |
| *trade-off* | compromiso, lo que ganas y pierdes |
| *coupling* / *decoupling* | acoplamiento / desacoplar |

**Gramática que confunde:** *eventual* es adjetivo (*eventual consistency*); *eventually* es adverbio (*replicas converge eventually*). Por eso: *"The service is **eventually** consistent: replicas converge **eventually**."*

**Falsos amigos frecuentes:**
- *eventually* = finalmente, con el tiempo (no "a veces").
- *actually* = en realidad (no "actualmente", que es *currently*).
- *library* = biblioteca (no librería, que sería *bookstore*); en programación decimos "librería" por costumbre, pero en inglés es *library*.
- *argument* = argumento de una función, o discusión.
- *to assist* = ayudar (no "asistir a"; eso es *to attend*).
- *carpet* = alfombra (carpeta es *folder*).

**Pronunciación común** (aproximada para hispanohablantes):
- *cache*: como "cash" (no "cach-é").
- *queue*: "kiú".
- *SQL*: "es-kiú-el" o "sí-kuel" (ambas se escuchan).
- *Azure*: suele pronunciarse "á-shur".
- *GUID*: "gú-id" o "guíd".
- *char*: "char" o "kar".

### Ejemplo
Traducción de una frase de un plan de despliegue de Itzamna.Dionisio:
> "Hay que hacer un despliegue gradual y, si falla, revertir."
> → *"We need to do a gradual rollout and, if it fails, roll back."*

Nota que *roll back* (verbo) va separado; *rollback* (sustantivo) va junto: *"The rollback took five minutes."* Lo mismo con *set up* / *setup*, *log in* / *login*, *shut down* / *shutdown*.

Mini diálogo en una revisión:
> *"This endpoint is the bottleneck: throughput drops when we hit 500 requests per second. If we add a cache, latency goes down, but the data will be eventually consistent. I think that trade-off is fine for the catalog, but not for prices."*

### Cuándo usarlo y cuándo no
Arma tu propia nota de vocabulario a partir de lo que lees en el trabajo y repásala cada semana ([[Inglés técnico - plan de estudio]]). En documentos en español con tu equipo, usar el término en inglés (*rollback*, *throughput*) está bien si todos lo entienden; no fuerces traducciones raras. En inglés, evita el *spanglish* técnico ("*to mergear*"); es *to merge*.

### Errores comunes
1. **Falsos amigos** (*actually*, *eventually*, *assist*).
2. **Pronunciar como se escribe** en español: "ca-ché", "que-ue".
3. **Confundir verbo y sustantivo** (*roll back* vs *rollback*, *set up* vs *setup*).
4. **Aprender listas de palabras sin contexto**; se olvidan en días.
5. **Usar *eventual* como "ocasional"**: en inglés significa "que ocurrirá al final".

### Para profundizar
- [RFC 2119: Key words for use in RFCs](https://www.rfc-editor.org/rfc/rfc2119): vocabulario de requisitos.
- [Technical Writing courses — Google for Developers](https://developers.google.com/tech-writing)
- [The System Design Primer](https://github.com/donnemartin/system-design-primer): mucho vocabulario de arquitectura en contexto.

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Completa: "The service is ___ consistent: replicas converge ___." (eventually / eventual)
> "The service is **eventually** consistent: replicas converge **eventually**." (*eventual consistency* como sustantivo).

> [!question]- Traduce: "Hay que hacer un despliegue gradual y, si falla, revertir."
> "We need to do a gradual rollout and, if it fails, roll back." (*roll back* verbo separado; *rollback* sustantivo).

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Traduce: *rollback*, *rollout*, *throughput*, *bottleneck*.
> *Rollback*: revertir a la versión anterior. *Rollout*: despliegue gradual. *Throughput*: rendimiento/caudal (trabajo por unidad de tiempo). *Bottleneck*: cuello de botella.

> [!question]- Falsos amigos frecuentes.
> *Eventually* = finalmente/con el tiempo (no "eventualmente" como "a veces"); *library* = biblioteca (no librería/tienda); *argument* = argumento de función o discusión; *carpet* ≠ carpeta (*folder*).

> [!question]- ¿Cómo se pronuncia *cache*, *queue* y *SQL* comúnmente?
> *Cache* como "cash"; *queue* como "kiú"; *SQL* como "es-kiú-el" o "sí-kuel".

## Práctica

- [[Lab 5 - Escribir un PR y un issue en inglés]]

