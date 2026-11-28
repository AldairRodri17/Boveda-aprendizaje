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
# CVE y CVSS

Parte de: [[12-Seguridad/06 Hacking ético/_Índice Hacking ético|Hacking ético]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**CVE** (*Common Vulnerabilities and Exposures*) es un catálogo público que asigna un identificador único a cada vulnerabilidad conocida, con el formato `CVE-AAAA-NNNNN` (año y número). **CVSS** (*Common Vulnerability Scoring System*) es un sistema estándar para puntuar la **gravedad** de una vulnerabilidad de 0.0 a 10.0. Juntos permiten que todo el mundo se refiera a la misma falla con el mismo nombre y mida su severidad de forma comparable. Como desarrollador, los ves en los avisos de dependencias ([[Componentes y dependencias vulnerables]]) y en los reportes de seguridad.

### CVE: identificar
Un **CVE** es solo un identificador y una descripción corta de una vulnerabilidad concreta en un producto. Lo asigna una **CNA** (*CVE Numbering Authority*), organización autorizada dentro del programa CVE (Microsoft, GitHub y muchos fabricantes son CNA). El CVE no dice qué tan grave es; para eso está el CVSS, y para el detalle técnico están bases como la NVD (*National Vulnerability Database*).

### CVSS: puntuar
El **CVSS** calcula una puntuación base a partir de métricas sobre cómo se explota y qué impacto tiene. En la versión 3.1, las métricas base incluyen:

- **AV** (*Attack Vector*): N (red), A (red adyacente), L (local), P (físico).
- **AC** (*Attack Complexity*): L (baja), H (alta).
- **PR** (*Privileges Required*): N (ninguno), L (bajos), H (altos).
- **UI** (*User Interaction*): N (ninguna), R (requerida).
- **C/I/A**: impacto en confidencialidad, integridad y disponibilidad (N/L/H).

El resultado se expresa como un **vector** (una cadena) y una puntuación. Por ejemplo, `AV:N/AC:L/PR:N/UI:N` significa: explotable **por red**, con **baja** complejidad, **sin** privilegios y **sin** interacción del usuario; es el perfil más peligroso. Si además el impacto en C, I y A es alto, la puntuación llega a **9.8** (crítica). Que una vulnerabilidad requiera una **cuenta de administrador** para explotarse se refleja en `PR:H` (privilegios altos), lo que baja la puntuación.

Rangos cualitativos de CVSS:

| Puntuación | Severidad |
|---|---|
| 0.0 | Ninguna |
| 0.1 – 3.9 | Baja |
| 4.0 – 6.9 | Media |
| 7.0 – 8.9 | Alta |
| 9.0 – 10.0 | Crítica |

Así, 3.1 es **baja**, 5.0 es **media**, 7.5 es **alta** y 9.8 es **crítica**. La versión **CVSS 4.0** se publicó en 2023 y refina el modelo, pero muchas fuentes siguen usando 3.1; revisa qué versión reporta cada aviso.

### ¿CVSS alto significa parchar primero?
No necesariamente. CVSS mide **gravedad técnica** en abstracto, no tu **riesgo real**. Para priorizar, combínalo con:

- **Tu exposición**: ¿el componente vulnerable está expuesto a internet o es interno? ¿usas la función afectada?
- **Explotación activa**: ¿se está explotando en la práctica? El catálogo **KEV** (*Known Exploited Vulnerabilities*) de CISA lista vulnerabilidades con explotación confirmada; el puntaje **EPSS** estima la probabilidad de que se explote pronto.

Una vulnerabilidad 9.8 en una librería que no usas o que no está expuesta puede ser menos urgente que una 6.5 en tu endpoint público bajo ataque activo.

### Ejemplo
Dependabot reporta dos CVE en Itzamna.Dionisio:

- CVE-A, CVSS 9.8 (`AV:N/AC:L/PR:N/UI:N`, impacto alto), en una librería que usas solo en una herramienta de línea de comandos interna no expuesta.
- CVE-B, CVSS 7.5, en una librería que procesa peticiones públicas de tu API.

Aunque CVE-A tiene mayor puntuación, revisas KEV/EPSS y tu exposición: CVE-B está en la ruta pública y la función se usa, así que lo parchas **primero** y luego CVE-A. El CVSS te orientó; la decisión la tomaste con contexto. Esta priorización se refleja también en [[Cómo escribir un reporte de vulnerabilidad]], donde el vector CVSS acompaña cada hallazgo.

### Cuándo usarlo y cuándo no
Usa CVSS para **comunicar** gravedad de forma estándar y como punto de partida de la priorización. No lo uses como único criterio para decidir el orden de parcheo: súmale exposición y explotación. El CVE es imprescindible para referirte a una falla sin ambigüedad.

### Errores comunes
- **Parchar estrictamente por puntuación** sin mirar exposición ni explotación.
- **Confundir CVE con CVSS**: uno identifica, el otro puntúa.
- **Ignorar la versión de CVSS** (3.1 vs 4.0) y comparar puntuaciones de distinta escala.
- **Pensar que "sin CVE" es "sin riesgo"**: no toda vulnerabilidad tiene CVE asignado.
- **Olvidar las métricas temporales y de entorno**: CVSS permite ajustar la base a tu contexto.

### Para profundizar
- [Programa CVE (sitio oficial)](https://www.cve.org/)
- [FIRST - CVSS (especificación y calculadora)](https://www.first.org/cvss/)
- [NVD - National Vulnerability Database](https://nvd.nist.gov/)
- [CISA - Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Clasifica según los rangos cualitativos de CVSS: 3.1, 5.0, 7.5, 9.8.
> 3.1 baja (0.1–3.9), 5.0 media (4.0–6.9), 7.5 alta (7.0–8.9), 9.8 crítica (9.0–10.0).

> [!question]- En CVSS 3.1, ¿qué significa `AV:N/AC:L/PR:N/UI:N`?
> Vector de ataque por red, complejidad baja, sin privilegios requeridos y sin interacción del usuario: lo peor en explotabilidad.

> [!question]- Una vulnerabilidad requiere una cuenta de administrador para explotarse. ¿Qué métrica lo refleja?
> Privilegios requeridos (`PR:H`), que baja la puntuación.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un CVE?
> Un identificador público único para una vulnerabilidad conocida (formato `CVE-AAAA-NNNNN`), asignado por una CNA dentro del programa CVE.

> [!question]- ¿Qué es CVSS?
> Common Vulnerability Scoring System: un sistema para puntuar la gravedad de 0.0 a 10.0 a partir de métricas como vector de ataque, complejidad, privilegios requeridos, interacción del usuario e impacto en confidencialidad, integridad y disponibilidad. La versión 4.0 se publicó en 2023; muchas fuentes aún usan 3.1.

> [!question]- ¿CVSS alto significa que debes parchar primero?
> No siempre. CVSS mide gravedad técnica; la prioridad también depende de tu exposición y de si se explota activamente (por ejemplo el catálogo KEV de CISA o el puntaje EPSS).

## Práctica

- [[Lab 11 - Carta de autorización y reporte de vulnerabilidad]]

