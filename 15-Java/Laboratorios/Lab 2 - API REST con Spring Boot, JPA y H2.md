---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Java
herramienta: JDK 21, Spring Initializr, Maven Wrapper, VS Code o IntelliJ IDEA Community
tags: [java, laboratorio]
---
# Lab 2 - API REST con Spring Boot, JPA y H2

Parte de: [[15-Java/_Índice Java|Java]]

**Cubre:** [[Spring Boot]] · [[Spring Boot - inyección de dependencias y beans]] · [[Spring Boot - controladores REST]] · [[Spring Boot - configuración y perfiles]] · [[JPA e Hibernate]] · [[Java - Maven y Gradle]] · [[Java - clases, interfaces y records]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Construir un CRUD de productos con Spring Boot, Spring Data JPA y base H2 en memoria, con validación, un perfil `dev` y pruebas, y compararlo con lo que harías en ASP.NET Core.

## Pasos
1. En start.spring.io genera un proyecto Maven, Java 21, con: Spring Web, Spring Data JPA, H2 Database y Validation.
2. Crea la entidad `Producto` (id, nombre, precio) y el repositorio `ProductoRepository extends JpaRepository`.
3. Crea un `record ProductoRequest` con `@NotBlank nombre` y `@Positive precio`.
4. Crea un servicio `ProductoService` inyectado por constructor en el controlador.
5. Implementa `GET /api/productos`, `GET /api/productos/{id}`, `POST /api/productos` (201 con header `Location`) y `DELETE /api/productos/{id}`.
6. En `application-dev.properties` activa la consola de H2 y el log de SQL; corre con el perfil `dev`.
7. Prueba con un archivo `.http` o curl.
8. Escribe una prueba con `@SpringBootTest` + `MockMvc` que verifique el 201 y una que verifique el 400 por precio negativo.

## Criterios de terminado
- [ ] `./mvnw spring-boot:run` arranca en el puerto 8080 con el perfil `dev`.
- [ ] `POST` con datos válidos devuelve **201** y el header `Location: /api/productos/{id}`.
- [ ] `POST` con `precio: -5` devuelve **400**.
- [ ] `GET /api/productos/999` devuelve **404**.
- [ ] El log muestra el SQL que generó Hibernate.
- [ ] `./mvnw test` pasa.
- [ ] Anotaste 3 equivalencias con ASP.NET Core + EF Core.

## Solución

> [!success]- Entidad, DTO y repositorio
> ```java
> @Entity
> public class Producto {
>     @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
>     private Long id;
>     private String nombre;
>     private BigDecimal precio;
>     protected Producto() {}
>     public Producto(String nombre, BigDecimal precio) { this.nombre = nombre; this.precio = precio; }
>     public Long getId() { return id; }
>     public String getNombre() { return nombre; }
>     public BigDecimal getPrecio() { return precio; }
> }
>
> public record ProductoRequest(@NotBlank String nombre, @NotNull @Positive BigDecimal precio) {}
>
> public interface ProductoRepository extends JpaRepository<Producto, Long> {}
> ```
> JPA necesita un constructor sin argumentos; por eso la entidad no es un `record`.

> [!success]- Controlador
> ```java
> @RestController
> @RequestMapping("/api/productos")
> public class ProductoController {
>     private final ProductoRepository repo;
>     public ProductoController(ProductoRepository repo) { this.repo = repo; }
>
>     @GetMapping
>     public List<Producto> todos() { return repo.findAll(); }
>
>     @GetMapping("/{id}")
>     public ResponseEntity<Producto> uno(@PathVariable Long id) {
>         return repo.findById(id).map(ResponseEntity::ok).orElse(ResponseEntity.notFound().build());
>     }
>
>     @PostMapping
>     public ResponseEntity<Producto> crear(@Valid @RequestBody ProductoRequest r, UriComponentsBuilder uri) {
>         var p = repo.save(new Producto(r.nombre(), r.precio()));
>         return ResponseEntity.created(uri.path("/api/productos/{id}").buildAndExpand(p.getId()).toUri()).body(p);
>     }
>
>     @DeleteMapping("/{id}")
>     public ResponseEntity<Void> borrar(@PathVariable Long id) {
>         if (!repo.existsById(id)) return ResponseEntity.notFound().build();
>         repo.deleteById(id);
>         return ResponseEntity.noContent().build();
>     }
> }
> ```
> Por brevedad se usa el repositorio directo; en el lab pon la lógica en `ProductoService`.

> [!success]- Perfil dev
> ```properties
> # src/main/resources/application-dev.properties
> spring.h2.console.enabled=true
> spring.jpa.show-sql=true
> ```
> ```powershell
> ./mvnw spring-boot:run "-Dspring-boot.run.profiles=dev"
> ```

> [!success]- Prueba con MockMvc
> ```java
> @SpringBootTest
> @AutoConfigureMockMvc
> class ProductoControllerTest {
>     @Autowired MockMvc mvc;
>
>     @Test
>     void crearDevuelve201() throws Exception {
>         mvc.perform(post("/api/productos").contentType(MediaType.APPLICATION_JSON)
>                 .content("{\"nombre\":\"Taza\",\"precio\":120}"))
>            .andExpect(status().isCreated())
>            .andExpect(header().exists("Location"));
>     }
>
>     @Test
>     void precioNegativoDevuelve400() throws Exception {
>         mvc.perform(post("/api/productos").contentType(MediaType.APPLICATION_JSON)
>                 .content("{\"nombre\":\"Taza\",\"precio\":-5}"))
>            .andExpect(status().isBadRequest());
>     }
> }
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
