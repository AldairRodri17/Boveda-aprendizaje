---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Java
aliases: ["Java vs C# (comparativa)"]
tags: [java]
contenido: completo
---
# Java vs CSharp (comparativa)

Parte de: [[15-Java/_Índice Java|Java]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Java y C# son lenguajes hermanos: ambos son orientados a objetos, con tipado estático, recolección de basura, código intermedio que ejecuta una máquina virtual con JIT (JVM y CLR), genéricos, lambdas y un ecosistema empresarial enorme. C# nació en 2000 muy inspirado en Java y desde entonces cada uno ha tomado ideas del otro. Para alguien que domina C# y .NET, aprender Java es sobre todo **aprender las diferencias** y el **mapa del ecosistema**. Esta nota es ese mapa; los detalles están en [[Java - sintaxis básica]], [[Java - clases, interfaces y records]] y [[Spring Boot]].

### Cómo funciona: diferencias de lenguaje
| Tema | Java | C# |
|---|---|---|
| Propiedades | No existen; getters/setters o records | `{ get; set; }`, `init`, `required` |
| Tipos de valor | Solo primitivos (`int`, `double`…); no hay `struct` de usuario | `struct`, `record struct`, `Span<T>` |
| Genéricos | **Borrado de tipos** (*type erasure*); no admite primitivos (`List<int>` no existe) | **Reificados**: el tipo existe en ejecución; `List<int>` sin boxing |
| Excepciones | **Verificadas** (`throws`) y no verificadas | Todas no verificadas |
| Asincronía | `CompletableFuture`, **hilos virtuales** (Java 21) | `async`/`await` y `Task` |
| Sobrecarga de operadores | No (por eso `String` se compara con `equals`) | Sí |
| Métodos | Virtuales por defecto | No virtuales por defecto |
| Nulabilidad | Sin anotaciones en el lenguaje; `Optional` y anotaciones de terceros | *Nullable reference types* (`string?`) |
| Consultas integradas | Streams (API de biblioteca) | LINQ (sintaxis del lenguaje + expresiones) |
| Extensiones | No hay métodos de extensión | Métodos de extensión |
| Eventos y delegados | Interfaces funcionales (`Function`, `Consumer`) | `delegate`, `event`, `Func`, `Action` |
| Verificación de nombres de archivo | Una clase pública por archivo con su nombre; paquetes = carpetas | Libre |

#### Type erasure
Al compilar, Java **quita** la información de los parámetros genéricos y la reemplaza por `Object` (o por el límite, como `Comparable`) con conversiones insertadas. En ejecución, `List<String>` y `List<Integer>` son **la misma clase** `List`. Esto se hizo para mantener compatibilidad con código anterior a Java 5. Consecuencias:

- No puedes hacer `new T()` ni `new T[10]`: el tipo no existe en ejecución. Se pasa un `Class<T>` o un `Supplier<T>`.
- No puedes preguntar `obj instanceof List<String>` (solo `instanceof List<?>`).
- No puedes sobrecargar `procesar(List<String>)` y `procesar(List<Integer>)`: tienen la misma firma borrada.
- No hay genéricos de primitivos: `List<Integer>` implica *boxing*.
- Frameworks como Jackson o Spring necesitan trucos (`TypeReference<List<Producto>>`, `ParameterizedTypeReference`) para saber el tipo concreto.

En C#, `typeof(T)`, `new T()` con restricción `new()` y `List<int>` funcionan porque los genéricos están en el runtime.

#### Hilos virtuales (Java 21)
Un **hilo virtual** es un hilo ligero administrado por la JVM, no por el sistema operativo. Puedes crear **millones** porque cuando uno se bloquea esperando E/S (una consulta a la base, una llamada HTTP), la JVM lo "estaciona" y libera el hilo del sistema operativo para otro trabajo. Ventaja: escribes código **bloqueante y secuencial**, fácil de leer y de depurar, y escala como código asíncrono. Es otra solución al mismo problema que C# resuelve con `async`/`await`, sin "colorear" las funciones (no hay que propagar `async` por toda la cadena).

```java
try (var ejecutor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (int i = 0; i < 10_000; i++) {
        int id = i;
        ejecutor.submit(() -> consultarApi(id)); // cada tarea en su hilo virtual; bloquear es barato
    }
}
```

En Spring Boot se activan con `spring.threads.virtual.enabled=true`, y cada petición HTTP se atiende en un hilo virtual.

### Equivalencias de ecosistema
| .NET | Java |
|---|---|
| CLR, IL, .NET SDK | JVM, bytecode, JDK ([[Java - JVM, JDK y JRE]]) |
| NuGet / nuget.org | Maven Central con Maven o Gradle ([[Java - Maven y Gradle]]) |
| `.csproj`, `dotnet` CLI | `pom.xml` / `build.gradle.kts`, `mvnw` / `gradlew` |
| ASP.NET Core (controllers) | Spring Boot con Spring Web MVC ([[Spring Boot - controladores REST]]) |
| `IServiceCollection` / DI | Contenedor de Spring ([[Spring Boot - inyección de dependencias y beans]]) |
| `appsettings.json` + entornos | `application.yml` + perfiles ([[Spring Boot - configuración y perfiles]]) |
| EF Core | JPA con Hibernate y Spring Data JPA ([[JPA e Hibernate]]) |
| Dapper | JdbcTemplate / JdbcClient de Spring, jOOQ, MyBatis |
| LINQ | Streams ([[Streams en Java (map, filter y collect)]]) |
| FluentValidation / DataAnnotations | Jakarta Bean Validation (`@NotBlank`, `@Valid`) |
| xUnit / NUnit, Moq | JUnit 5, Mockito, AssertJ |
| Serilog / ILogger | SLF4J con Logback o Log4j2 |
| System.Text.Json | Jackson |
| Visual Studio / Rider | IntelliJ IDEA, VS Code con Extension Pack for Java, Eclipse |

### Ejemplo: el mismo endpoint
```csharp
// C# — ASP.NET Core
[ApiController, Route("api/productos")]
public class ProductosController(IProductoRepo repo) : ControllerBase {
    [HttpGet("{id:long}")]
    public async Task<ActionResult<ProductoDto>> Uno(long id) =>
        await repo.Obtener(id) is { } p ? Ok(p) : NotFound();
}
```

```java
// Java — Spring Boot
@RestController
@RequestMapping("/api/productos")
class ProductosController {
    private final ProductoRepo repo;
    ProductosController(ProductoRepo repo) { this.repo = repo; }

    @GetMapping("/{id}")
    ResponseEntity<ProductoDto> uno(@PathVariable long id) {
        return repo.obtener(id).map(ResponseEntity::ok).orElse(ResponseEntity.notFound().build());
    }
}
```

La estructura es casi la misma; cambia la sintaxis de anotaciones y que en Java el código es síncrono (con hilos virtuales escala igual).

### Cuándo usar cada uno
No es "cuál es mejor": depende del equipo y el entorno. Java domina en banca, gobierno, telecomunicaciones, Android y en mucho software empresarial heredado; .NET es fuerte en empresas Microsoft y Azure. Aprender Java te abre ese mercado y te permite leer y mantener servicios Java en arquitecturas mixtas.

### Errores comunes al pasar de C# a Java
- Comparar cadenas con `==`.
- Buscar propiedades y escribir campos públicos.
- Esperar `async`/`await` y bloquearse con `CompletableFuture` cuando bastaba código bloqueante con hilos virtuales.
- Intentar `new T()` o `typeof(T)` en genéricos.
- Ignorar las excepciones verificadas y envolverlo todo en `RuntimeException` sin criterio.

### Para profundizar
- [Hilos virtuales (dev.java)](https://dev.java/learn/api/virtual-threads/)
- [JEP 444: Virtual Threads (OpenJDK)](https://openjdk.org/jeps/444)
- [Genéricos y type erasure (tutorial de Java, Oracle)](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html)
- [Java en Azure (Microsoft Learn)](https://learn.microsoft.com/azure/developer/java/)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Menciona cinco diferencias de lenguaje entre Java y C#.
> 1. C# tiene propiedades; Java usa getters/setters (o records).
> 2. C# tiene `struct` y tipos de valor propios; Java solo primitivos.
> 3. Genéricos: en Java se borran en tiempo de ejecución (type erasure); en C# se conservan (`typeof(T)` funciona).
> 4. Java tiene excepciones verificadas (`throws`); C# no.
> 5. C# tiene `async/await`; Java usa `CompletableFuture` e hilos virtuales (Java 21).

> [!question]- ¿Qué ventaja tienen los hilos virtuales de Java 21?
> Permiten escribir código bloqueante sencillo que escala a muchísimas tareas concurrentes, porque los hilos virtuales son baratos. Es otra forma de resolver lo que en C# resuelve `async/await`.

> [!question]- ¿Qué equivale a LINQ, EF Core, NuGet y ASP.NET Core en Java?
> Streams, JPA/Hibernate (Spring Data JPA), Maven Central (con Maven o Gradle) y Spring Boot.

> [!question]- ¿Qué es type erasure y qué limita?
> El compilador quita la información de tipos genéricos al generar bytecode: `List<String>` y `List<Integer>` son la misma clase en ejecución. Por eso no puedes hacer `new T()` ni `instanceof List<String>`.

## Práctica

- [[Lab 3 - Colecciones y streams de Java comparados con LINQ]]

