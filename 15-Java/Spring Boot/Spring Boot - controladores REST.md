---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: Java
tags: [java]
---
# Spring Boot - controladores REST

Parte de: [[Spring Boot]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Un controlador REST en Spring Boot es una clase anotada con **`@RestController`** cuyos métodos atienden peticiones HTTP y devuelven datos que Spring serializa a JSON (con Jackson). Pertenece a **Spring Web MVC**: el `DispatcherServlet` recibe cada petición, busca el método cuyo mapeo coincide (ruta + verbo), convierte parámetros y cuerpo a objetos Java, invoca el método y escribe la respuesta. Es el mismo modelo que los **controllers de ASP.NET Core** con `[ApiController]`.

### Cómo funciona
```mermaid
sequenceDiagram
    participant C as Cliente
    participant D as DispatcherServlet
    participant H as ProductoController
    participant S as Servicio / Repositorio
    C->>D: POST /api/productos (JSON)
    D->>D: Busca el handler por ruta y verbo
    D->>D: Jackson: JSON → ProductoNuevo; @Valid valida
    alt inválido
        D-->>C: 400 + ProblemDetail
    else válido
        D->>H: crear(dto)
        H->>S: guardar
        S-->>H: Producto
        H-->>D: ResponseEntity 201 + Location
        D-->>C: 201 Created (JSON)
    end
```

#### Equivalencias de anotaciones
| Spring | ASP.NET Core |
|---|---|
| `@RestController` | `[ApiController]` + `ControllerBase` |
| `@RequestMapping("/api/productos")` en la clase | `[Route("api/productos")]` |
| `@GetMapping`, `@PostMapping`, `@PutMapping`, `@PatchMapping`, `@DeleteMapping` | `[HttpGet]`, `[HttpPost]`, `[HttpPut]`, `[HttpPatch]`, `[HttpDelete]` |
| `@PathVariable` | Parámetro de ruta `{id}` |
| `@RequestParam` | `[FromQuery]` |
| `@RequestBody` | `[FromBody]` |
| `@RequestHeader` | `[FromHeader]` |
| `ResponseEntity<T>` | `ActionResult<T>` / `IActionResult` / `Results` |
| `@ResponseStatus(HttpStatus.CREATED)` | `StatusCode(201)` |
| `@RestControllerAdvice` + `@ExceptionHandler` | Filtro de excepciones / middleware `UseExceptionHandler` |
| `ProblemDetail` | `ProblemDetails` |

#### Validación del cuerpo
Agrega el starter `spring-boot-starter-validation` (Jakarta Bean Validation, implementado por Hibernate Validator). Pones restricciones en el DTO —`@NotBlank`, `@Size`, `@Positive`, `@Email`, `@NotNull`, `@Pattern`— y marcas el parámetro con **`@Valid @RequestBody`**. Si falla, Spring lanza `MethodArgumentNotValidException` y responde **400** antes de entrar a tu método. Es el equivalente de DataAnnotations con `[ApiController]`; si estás acostumbrado a FluentValidation, la alternativa en Java es escribir un validador propio en el servicio o usar `@Validated` con grupos, pero lo idiomático son las anotaciones.

#### Errores con ProblemDetail
Spring implementa el formato **RFC 9457** (*Problem Details*) con la clase `ProblemDetail`. Puedes activar las respuestas estándar con `spring.mvc.problemdetails.enabled=true` y, para tu convención de incluir un `codigo`, crear un `@RestControllerAdvice` que agregue propiedades con `setProperty("codigo", ...)`.

### Ejemplo completo
```java
package mx.itzamna.catalogo.productos;

import jakarta.validation.Valid;
import jakarta.validation.constraints.*;
import java.math.BigDecimal;
import java.net.URI;
import java.util.List;
import org.springframework.http.*;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.*;

record ProductoNuevo(
    @NotBlank @Size(max = 120) String nombre,
    @NotNull @Positive BigDecimal precio) {}

record ProductoDto(long id, String nombre, BigDecimal precio) {}

@RestController
@RequestMapping("/api/productos")
class ProductoController {
    private final ProductoService servicio;

    ProductoController(ProductoService servicio) { this.servicio = servicio; }

    @GetMapping
    List<ProductoDto> listar(@RequestParam(defaultValue = "") String q,
                             @RequestParam(defaultValue = "0") int pagina) {
        return servicio.buscar(q, pagina);
    }

    @GetMapping("/{id}")
    ResponseEntity<ProductoDto> uno(@PathVariable long id) {
        return servicio.obtener(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    ResponseEntity<ProductoDto> crear(@Valid @RequestBody ProductoNuevo nuevo) {
        var creado = servicio.crear(nuevo);
        return ResponseEntity.created(URI.create("/api/productos/" + creado.id())).body(creado);
    }

    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    void borrar(@PathVariable long id) { servicio.borrar(id); }
}

// Manejo centralizado de errores, con "codigo" como en tus APIs .NET
@RestControllerAdvice
class ManejadorErrores {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    ProblemDetail validacion(MethodArgumentNotValidException ex) {
        var pd = ProblemDetail.forStatusAndDetail(HttpStatus.BAD_REQUEST, "Datos inválidos");
        pd.setProperty("codigo", "VALIDACION");
        pd.setProperty("errores", ex.getBindingResult().getFieldErrors().stream()
            .map(e -> e.getField() + ": " + e.getDefaultMessage()).toList());
        return pd;
    }

    @ExceptionHandler(ProductoNoEncontrado.class)
    ProblemDetail noEncontrado(ProductoNoEncontrado ex) {
        var pd = ProblemDetail.forStatusAndDetail(HttpStatus.NOT_FOUND, ex.getMessage());
        pd.setProperty("codigo", "PRODUCTO_NO_ENCONTRADO");
        return pd;
    }
}
```

(`ProductoService` y `ProductoNoEncontrado` son tuyos: el servicio es un bean, ver [[Spring Boot - inyección de dependencias y beans]].) Prueba rápida:

```bash
curl -i -X POST http://localhost:8080/api/productos \
  -H "Content-Type: application/json" -d '{"nombre":"","precio":-5}'
# 400 con application/problem+json, codigo VALIDACION y la lista de errores
```

Para pruebas automatizadas, `@WebMvcTest(ProductoController.class)` levanta solo la capa web con `MockMvc` y el servicio simulado, parecido a probar un controller con `WebApplicationFactory` y servicios reemplazados.

### Cuándo usarlo y cuándo no
Usa controladores con anotaciones para APIs REST tradicionales: es lo más común y lo que verás en el trabajo. Spring también ofrece *functional endpoints* (`RouterFunction`), parecidos a las Minimal APIs, y **WebFlux** para programación reactiva; con hilos virtuales (Java 21) el modelo MVC bloqueante escala bien y es más sencillo, así que WebFlux solo vale la pena si ya trabajas con flujos reactivos. Devuelve **DTOs** (records), no entidades JPA: evitas exponer campos internos y problemas de carga perezosa al serializar.

### Errores comunes
- **Olvidar `@Valid`**: las anotaciones del DTO no se evalúan.
- **Olvidar el starter de validación**: las anotaciones existen pero nadie las ejecuta.
- **Devolver entidades JPA**: ciclos infinitos en JSON y `LazyInitializationException`.
- **Usar `@Controller` en vez de `@RestController`**: Spring busca una vista en lugar de serializar JSON (falta `@ResponseBody`).
- **Lógica de negocio en el controlador**: ponla en servicios o casos de uso por *slice*, como en tu arquitectura .NET.

### Para profundizar
- [Construir un servicio REST (guía de Spring)](https://spring.io/guides/gs/rest-service/)
- [Controladores anotados de Spring MVC (referencia)](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller.html)
- [Respuestas de error con ProblemDetail (Spring MVC)](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-rest-exceptions.html)
- [Validación en Spring Boot (referencia)](https://docs.spring.io/spring-boot/reference/io/validation.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- Escribe un controlador mínimo.
> ```java
> @RestController
> @RequestMapping("/api/productos")
> class ProductoController {
>     private final ProductoRepository repo;
>     ProductoController(ProductoRepository repo) { this.repo = repo; }
>
>     @GetMapping("/{id}")
>     ResponseEntity<Producto> uno(@PathVariable Long id) {
>         return repo.findById(id).map(ResponseEntity::ok)
>                    .orElse(ResponseEntity.notFound().build());
>     }
> }
> ```

> [!question]- Equivalencias de anotaciones con ASP.NET Core.
> `@RestController` ≈ `[ApiController]`, `@GetMapping` ≈ `[HttpGet]`, `@PathVariable` ≈ parámetro de ruta, `@RequestBody` ≈ `[FromBody]`, `@RequestParam` ≈ `[FromQuery]`, `ResponseEntity` ≈ `IActionResult`/`Results`.

> [!question]- ¿Cómo validas el cuerpo de una petición?
> Con el starter de validación (Jakarta Bean Validation): anotaciones como `@NotBlank` y `@Positive` en el DTO y `@Valid @RequestBody` en el parámetro. Si falla, Spring responde 400.

## Práctica

- [[Lab 2 - API REST con Spring Boot, JPA y H2]]

