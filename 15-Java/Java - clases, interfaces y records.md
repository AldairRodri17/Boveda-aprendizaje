---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Java
tags: [java]
---
# Java - clases, interfaces y records

Parte de: [[15-Java/_Índice Java|Java]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Como en C#, en Java todo vive dentro de **tipos**: clases, interfaces, enums y, desde Java 16, **records**. Las ideas son las mismas (encapsulamiento, herencia simple de clases, implementación múltiple de interfaces, polimorfismo), pero hay diferencias que cambian cómo escribes el código: no hay propiedades, los métodos son virtuales por defecto, las interfaces pueden tener implementación (`default`), y las jerarquías se pueden cerrar con `sealed`. Esta nota te da el mapa para traducir tus hábitos de C#.

### Cómo funciona
#### Clases: campos, constructores y "propiedades"
Java **no tiene propiedades** (`{ get; set; }`). Se usan campos privados con métodos `getX()`/`setX()` por convención (la convención *JavaBeans*, que frameworks como JPA y Jackson reconocen). Mucho código usa Lombok (`@Getter`, `@Setter`) para no escribirlos, pero para datos inmutables lo moderno es un `record`.

```java
public class Cuenta {
    private final String numero;        // final ≈ readonly
    private BigDecimal saldo = BigDecimal.ZERO;

    public Cuenta(String numero) {
        if (numero == null || numero.isBlank()) throw new IllegalArgumentException("numero");
        this.numero = numero;
    }

    public String getNumero() { return numero; }
    public BigDecimal getSaldo() { return saldo; }

    public void depositar(BigDecimal monto) {
        if (monto.signum() <= 0) throw new IllegalArgumentException("monto debe ser positivo");
        saldo = saldo.add(monto);
    }

    @Override
    public String toString() { return "Cuenta[" + numero + ", " + saldo + "]"; }
}
```

Modificadores de acceso: `public`, `protected` (en Java incluye **todo el paquete**, además de subclases), `private` y el acceso por defecto sin palabra clave (*package-private*, visible solo en el mismo paquete), que es lo más parecido a `internal`.

#### Diferencias clave con C#
| Tema | Java | C# |
|---|---|---|
| Propiedades | Getters/setters o records | `{ get; set; }` |
| Métodos virtuales | **Virtuales por defecto**; `final` impide sobrescribir | No virtuales por defecto; `virtual`/`override` |
| Sobrescribir | Anotación `@Override` (opcional pero recomendada) | Palabra clave `override` obligatoria |
| Clase no heredable | `final class` | `sealed class` |
| Jerarquía cerrada | `sealed ... permits` | No hay equivalente directo |
| Tipos de valor propios | No hay `struct` de usuario (proyecto Valhalla en desarrollo) | `struct`, `record struct` |
| Herencia | `extends` (clase), `implements` (interfaces) | `:` para ambos |
| Llamar al padre | `super(...)`, `super.metodo()` | `base(...)`, `base.Metodo()` |
| Clases anidadas | `static class` (como en C#) o *inner class* que captura la instancia externa | Solo como `static` en la práctica |
| Igualdad | Sobrescribir `equals` **y** `hashCode` | `Equals` y `GetHashCode` |

#### Interfaces y métodos `default`
Una interfaz define un contrato. Desde Java 8 puede incluir métodos **`default`** (con implementación que las clases heredan y pueden sobrescribir), métodos `static` y, desde Java 9, métodos `private` de apoyo. Equivalen a los *default interface methods* de C# 8. Sirven para **evolucionar** una interfaz sin romper a quienes ya la implementan.

```java
public interface Notificador {
    void enviar(String destino, String mensaje);

    default void enviarATodos(List<String> destinos, String mensaje) {
        destinos.forEach(d -> enviar(d, mensaje));
    }
}
```

Una interfaz con un solo método abstracto es una **interfaz funcional** y se puede implementar con una lambda: `Notificador n = (d, m) -> System.out.println(d + ": " + m);`. Es como asignar una lambda a un delegado en C#; `Function<T,R>`, `Predicate<T>` y `Supplier<T>` equivalen a `Func` y `Predicate`.

#### Records
Un `record` es una clase **inmutable** y concisa para transportar datos:

```java
public record ProductoDto(long id, String nombre, BigDecimal precio) {
    // Constructor compacto: validación sin repetir parámetros
    public ProductoDto {
        if (precio.signum() < 0) throw new IllegalArgumentException("precio negativo");
        nombre = nombre.trim();
    }
    public boolean esCaro() { return precio.compareTo(new BigDecimal("1000")) > 0; }
}

var p = new ProductoDto(1, " Teclado ", new BigDecimal("1299"));
System.out.println(p.nombre());   // "Teclado": accesor sin prefijo get
System.out.println(p);            // ProductoDto[id=1, nombre=Teclado, precio=1299]
```

Genera constructor canónico, accesores `id()`, `nombre()`, `precio()`, `equals`, `hashCode` y `toString` por valor. Es equivalente a un `record` posicional de C#, con diferencias: los campos son siempre `final`, no hay expresión `with` (creas uno nuevo), un record no puede heredar de otra clase (sí implementar interfaces) y los accesores no llevan `get`. Son ideales para DTOs de una API, resultados de consultas y mensajes.

#### Clases e interfaces `sealed`
Desde Java 17, `sealed` limita **quién puede extender** un tipo con `permits`. Las subclases deben ser `final`, `sealed` o `non-sealed`. Combinado con `switch` de patrones (Java 21), el compilador verifica que cubriste **todos** los casos, sin `default`:

```java
public sealed interface Resultado permits Exito, Fallo {}
public record Exito(String folio) implements Resultado {}
public record Fallo(String codigo, String detalle) implements Resultado {}

static String describir(Resultado r) {
    return switch (r) {
        case Exito e -> "Creado " + e.folio();
        case Fallo(String codigo, String detalle) -> "Error " + codigo + ": " + detalle; // patrón de record
    };
}
```

Es la forma idiomática de modelar uniones discriminadas, como el resultado de un caso de uso con un `codigo` de error parecido al que pones en tus ProblemDetails.

### Cuándo usar cada uno
- **record**: datos inmutables (DTOs, valores, eventos).
- **class**: entidades con identidad y comportamiento, objetos con estado mutable controlado, entidades JPA (que necesitan constructor sin argumentos y no pueden ser records).
- **interface**: contratos y puntos de extensión (servicios inyectables en [[Spring Boot - inyección de dependencias y beans]]).
- **sealed**: cuando el conjunto de variantes es cerrado y quieres exhaustividad.
- **enum**: conjuntos fijos de constantes; en Java los enums pueden tener campos y métodos, más potentes que en C#.

### Errores comunes
- **Sobrescribir `equals` sin `hashCode`**: falla en `HashMap` y `HashSet`. Los records lo resuelven.
- **Olvidar que los métodos son virtuales**: un método llamado desde el constructor puede ejecutar la versión de la subclase antes de que esta se inicialice.
- **Usar records como entidades JPA**: no funcionan como entidades mutables.
- **Omitir `@Override`**: un error de firma crea un método nuevo en vez de sobrescribir.
- **Exponer campos mutables desde un record** (una `List` modificable): copia con `List.copyOf` en el constructor compacto.

### Para profundizar
- [Programación orientada a objetos en Java (dev.java)](https://dev.java/learn/language/oop/)
- [JEP 395: Records (OpenJDK)](https://openjdk.org/jeps/395)
- [JEP 409: Sealed Classes (OpenJDK)](https://openjdk.org/jeps/409)
- [Métodos default en interfaces (tutorial de Java, Oracle)](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es un `record` en Java?
> Una clase inmutable y concisa para datos (desde Java 16): `record Punto(int x, int y) {}` genera constructor, accesores `x()` y `y()`, `equals`, `hashCode` y `toString`. Equivale a un `record` posicional de C#.

> [!question]- ¿Diferencias clave con C# en clases?
> Java no tiene propiedades (`get/set`), se usan métodos `getX()`; no hay `struct` definidos por el usuario (por ahora); los métodos son virtuales por defecto (en C# hay que marcar `virtual`); `final` ≈ `sealed` para clases.

> [!question]- ¿Qué son los métodos `default` en interfaces?
> Métodos con implementación dentro de la interfaz (desde Java 8), equivalentes a los default interface methods de C# 8.

> [!question]- ¿Qué son las clases `sealed` en Java?
> Clases o interfaces que limitan qué tipos pueden extenderlas con `permits` (desde Java 17). Útiles con `switch` de patrones para que el compilador verifique que cubriste todos los casos.

## Práctica

- [[Lab 2 - API REST con Spring Boot, JPA y H2]]

