---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Fundamentos
tags: [fundamentos]
---
# Markdown

Parte de: [[02-Fundamentos/08 Computación general/_Índice Computación general|Computación general]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Markdown** es un lenguaje de marcado ligero creado por John Gruber en 2004 para escribir texto con formato usando solo caracteres simples, legible incluso sin renderizar. Se convierte a HTML y es el estándar de facto para README, documentación técnica, wikis de Azure DevOps, descripciones de PR, issues y notas como las de esta bóveda de Obsidian.

Como la especificación original era ambigua, surgió **CommonMark** (una especificación estricta) y sobre ella **GitHub Flavored Markdown (GFM)**, que agrega tablas, listas de tareas, tachado y autolinks. Muchas herramientas, incluida Obsidian, parten de CommonMark/GFM y agregan extensiones propias.

### Cómo funciona
Sintaxis base:

````markdown
 # Título de nivel 1
 ## Nivel 2
### Nivel 3

Párrafo con **negrita**, *cursiva*, `código en línea` y ~~tachado~~ (GFM).

- Lista con viñetas
  - Sublista (sangría de 2 o 4 espacios)
1. Lista numerada
2. Segundo elemento

- [ ] Tarea pendiente (GFM)
- [x] Tarea hecha

[Texto del enlace](https://learn.microsoft.com)
![Texto alternativo](img/diagrama.png)

> Cita en bloque

```csharp
// Bloque de código con lenguaje para resaltado de sintaxis
Console.WriteLine("Hola");
```

---
````

Una **línea en blanco** separa párrafos; un salto de línea simple dentro de un párrafo no siempre se respeta (depende del renderizador; en Obsidian hay una opción para respetarlos).

**Tablas** (GFM): filas con `|`, y una línea de guiones bajo el encabezado. Los dos puntos controlan la alineación:

```markdown
| Proyecto          | Stack         | Base de datos |
|:------------------|:-------------:|--------------:|
| Itzamna.Dionisio  | .NET + Dapper | SQL Server    |
| Pochteca.Nomisma  | .NET          | PostgreSQL    |
```

`:---` alinea a la izquierda, `:---:` al centro, `---:` a la derecha. No necesitas alinear las columnas en el texto, pero ayuda a leer.

**Lo que agrega Obsidian** (no es Markdown estándar y no se verá igual en GitHub o Azure DevOps):
- **Wikilinks**: `[[Nombre de nota]]`, `[[Nota|texto visible]]`, `[[Nota#Encabezado]]`, `[[Nota#^bloque]]`.
- **Incrustar** (transclusión): `![[Nota]]` o `![[imagen.png]]`.
- **Callouts**: `> [!info]`, `> [!question]-` (el `-` lo hace plegable), como los de la autoevaluación de esta nota.
- **Propiedades** en frontmatter YAML entre `---` al inicio (las que usa la bóveda: `progreso`, `nivel`, `tags`).
- **Etiquetas** `#tag`, **resaltado** `==texto==`, **comentarios** `%% oculto %%`.
- **Diagramas Mermaid** en bloques de código marcados con el lenguaje `mermaid`, y matemáticas con `$...$` (LaTeX/MathJax). Mermaid también lo soportan GitHub y las wikis de Azure DevOps, con diferencias de versión.

### Ejemplo
README mínimo para Ocelotl.Iris en Azure Repos:

````markdown
 # Ocelotl.Iris

Servicio de notificaciones push. API ASP.NET Core (.NET 10).

 ## Requisitos
- .NET 10 SDK
- Variables de entorno: `Iris__ColaUrl`, `Iris__Proveedor` (ver `.env.example`)

 ## Ejecutar
```bash
dotnet run --project src/Ocelotl.Iris.Api
```

 ## Decisiones
| Fecha      | Decisión                         | ADR |
|------------|----------------------------------|-----|
| 2026-10-02 | Cola en Azure Service Bus        | [0003](docs/adr/0003-service-bus.md) |
````

### Cuándo usarlo y cuándo no
- Ideal para documentación que vive **junto al código** y se revisa en PR (README, ADR, guías), y para notas personales.
- Si una nota de Obsidian se va a publicar en un repo, evita wikilinks y callouts o conviértelos a enlaces normales.
- Para documentos con maquetación compleja (contratos, reportes formales con encabezados de página) es mejor otra herramienta o exportar a PDF.

### Errores comunes
- **Olvidar la línea en blanco** antes de una lista o tabla: no se renderiza.
- **Sangría inconsistente** en sublistas: se "aplanan" o se vuelven bloques de código (4 espacios al inicio de párrafo = código en Markdown clásico).
- **No indicar el lenguaje** en bloques de código: pierdes resaltado.
- **Asumir que las extensiones de Obsidian funcionan en todos lados**: `[[...]]` en GitHub se ve como texto.
- **Usar `|` dentro de una celda** sin escaparlo (`\|`): rompe la tabla.

### Para profundizar
- [CommonMark: guía rápida y tutorial](https://commonmark.org/help/)
- [Especificación de GitHub Flavored Markdown](https://github.github.com/gfm/)
- [Sintaxis de formato en Obsidian (ayuda oficial)](https://help.obsidian.md/syntax)
- [Mermaid: documentación](https://mermaid.js.org/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se escriben encabezados, listas, enlaces y código en Markdown?
> `# Título`, `## Subtítulo`; listas con `-` o `1.`; enlaces `[texto](url)`; código en línea con comillas invertidas y bloques con tres comillas invertidas y el lenguaje (```` ```csharp ````).

> [!question]- ¿Qué diferencia hay entre Markdown estándar y lo que usa Obsidian?
> Obsidian agrega enlaces internos `[[Nota]]`, incrustaciones `![[Nota]]`, callouts `> [!info]`, propiedades en frontmatter YAML y etiquetas `#tag`. GitHub tiene su propio sabor (GFM): tablas, casillas `- [ ]` y alertas.

> [!question]- ¿Cómo haces una tabla?
> ```
> | Método | Idempotente |
> |---|---|
> | GET | Sí |
> | POST | No |
> ```

