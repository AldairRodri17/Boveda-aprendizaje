---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: Java
tags: [java]
---
# Java - sintaxis básica

Parte de: [[15-Java/_Índice Java|Java]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La sintaxis de Java te va a resultar muy familiar porque C# nació inspirado en ella: llaves, punto y coma, `if`/`for`/`while`, clases, `static`, `new`, tipos estáticos. Las diferencias importantes no están en la forma sino en el **modelo**: Java no tiene propiedades ni sobrecarga de operadores, distingue entre **tipos primitivos** y objetos de una manera más visible que C#, compara cadenas de forma distinta y tiene excepciones verificadas. Esta nota cubre lo mínimo para leer y escribir código Java con soltura.

### Cómo funciona
#### Estructura mínima de un programa
```java
// Archivo: Hola.java (debe llamarse igual que la clase pública)
package mx.xolotl.demo;           // como namespace, pero ligado a carpetas: mx/xolotl/demo/

import java.util.List;           // como using

public class Hola {
    public static void main(String[] args) {
        System.out.println("Hola, " + (args.length > 0 ? args[0] : "mundo"));
    }
}
```

Reglas: una clase `public` por archivo y el archivo se llama como ella; el `package` debe coincidir con la ruta de carpetas. El punto de entrada es `public static void main(String[] args)`. Desde Java 25 existe una forma compacta para programas pequeños (un archivo con `void main() { ... }` sin declarar la clase), útil para aprender o para scripts; en proyectos normales seguirás viendo la forma clásica.

#### Tipos primitivos y wrappers
| Primitivo | Tamaño | Wrapper | C# |
|---|---|---|---|
| `byte` | 8 bits (con signo) | `Byte` | `sbyte` |
| `short` | 16 | `Short` | `short` |
| `int` | 32 | `Integer` | `int` |
| `long` | 64 | `Long` | `long` |
| `float` / `double` | 32 / 64 | `Float` / `Double` | `float` / `double` |
| `char` | 16 (UTF-16) | `Character` | `char` |
| `boolean` | — | `Boolean` | `bool` |

Los primitivos guardan el valor directo y no pueden ser `null`. Los **wrappers** son objetos que los envuelven. Las colecciones y los genéricos solo trabajan con objetos (`List<Integer>`, nunca `List<int>`), así que Java convierte automáticamente (*autoboxing* y *unboxing*). Dos trampas: un `Integer` puede ser `null` y al desempacarlo lanza `NullPointerException`; y `==` entre dos `Integer` compara **referencias** (funciona "por casualidad" entre -128 y 127 por una caché interna, y falla fuera de ese rango). Para dinero no uses `double`: usa `BigDecimal` (el `decimal` de C#).

#### Cadenas: `==` contra `equals`
En Java, `==` en objetos compara **referencias** (¿es el mismo objeto?). Para comparar contenido se usa `.equals()`. En C# `==` sobre `string` compara contenido porque el operador está sobrecargado; Java no permite sobrecargar operadores.

```java
String a = "hola";
String b = new String("hola");
System.out.println(a == b);            // false: objetos distintos
System.out.println(a.equals(b));       // true: mismo contenido
System.out.println("hola".equalsIgnoreCase("HOLA")); // true
System.out.println(java.util.Objects.equals(null, a)); // false, sin NPE
```

Las cadenas son inmutables, como en C#. Para armar cadenas en ciclos usa `StringBuilder`. Desde Java 15 hay *text blocks* con `"""`, parecidos a los raw string literals de C#.

#### `var`, operadores y conversiones
`var` (desde Java 10) infiere el tipo de **variables locales**, igual que en C#; el tipo sigue siendo estático. No se puede usar en campos ni en parámetros de métodos. La división entre enteros es entera: `7 / 2` da `3`; si un operando es `double`, el resultado es decimal: `7 / 2.0` da `3.5`. El operador `+` con una cadena concatena de izquierda a derecha, así que `1 + 2 + "x"` es `"3x"` pero `"x" + 1 + 2` es `"x12"`.

#### Control de flujo y `switch` moderno
`if`, `for`, `while`, `do-while` y el for-each `for (var x : lista)` (equivale a `foreach`). El `switch` moderno (Java 14+) usa flechas, no necesita `break`, puede devolver un valor y, desde Java 21, acepta **patrones**:

```java
String tipo = switch (dia) {
    case 1, 2, 3, 4, 5 -> "hábil";
    case 6, 7 -> "fin de semana";
    default -> throw new IllegalArgumentException("Día inválido: " + dia);
};

Object valor = 42;
String descripcion = switch (valor) {
    case Integer i when i > 0 -> "entero positivo " + i;
    case Integer i -> "entero " + i;
    case String s -> "texto de " + s.length() + " caracteres";
    default -> "otro";
};
```

Equivale a las *switch expressions* y *pattern matching* de C#. `instanceof` también tiene patrón: `if (obj instanceof String s) { ... }`, como `obj is string s`.

#### Excepciones verificadas
Java distingue **checked exceptions** (heredan de `Exception` pero no de `RuntimeException`, como `IOException`): el compilador te obliga a capturarlas o declararlas con `throws` en la firma. C# no tiene este concepto. Las *unchecked* (`RuntimeException`, `IllegalArgumentException`, `NullPointerException`) funcionan como en C#. `try-with-resources` equivale a `using`:

```java
try (var lector = Files.newBufferedReader(Path.of("datos.csv"))) {
    System.out.println(lector.readLine());
} catch (IOException e) {
    System.err.println("No se pudo leer: " + e.getMessage());
}
```

### Ejemplo integrado
```java
import java.math.BigDecimal;
import java.util.List;

public class Ticket {
    public static void main(String[] args) {
        var precios = List.of(new BigDecimal("199.90"), new BigDecimal("45.50"));
        var total = BigDecimal.ZERO;
        for (var p : precios) total = total.add(p);       // BigDecimal es inmutable
        System.out.printf("Total: %s MXN%n", total);       // 245.40
        System.out.println(7 / 2 + " " + 7 / 2.0);         // 3 3.5
    }
}
```

### Cuándo usarlo y cuándo no
Esta sintaxis es la base de todo lo demás: [[Java - clases, interfaces y records]], [[Java - colecciones y streams]] y [[Spring Boot]]. Practica con `jshell` para probar expresiones sin crear proyectos. Para comparaciones más amplias con C#, ver [[Java vs CSharp (comparativa)]].

### Errores comunes
- **Comparar `String` o `Integer` con `==`**: usa `equals`.
- **`double` para dinero**: errores de redondeo; usa `BigDecimal` y créalo desde `String`.
- **Desempacar un wrapper `null`**: `NullPointerException`.
- **Olvidar que `7 / 2` es entero**.
- **Nombre de archivo distinto a la clase pública**: error de compilación.

### Para profundizar
- [Aprende Java: bases del lenguaje (dev.java)](https://dev.java/learn/language/constructs/)
- [Tutorial oficial de Java: lo básico del lenguaje (Oracle)](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/index.html)
- [Especificación del lenguaje Java (Oracle)](https://docs.oracle.com/javase/specs/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Qué imprime?
```java
String a = "hola";
String b = new String("hola");
System.out.println(a == b);
System.out.println(a.equals(b));
```
> `false` y `true`: `new String` crea otro objeto, pero el contenido es igual.

> [!question]- Escribe un `switch` moderno que devuelva el nombre del día para 1–7.
> ```java
> String nombre = switch (dia) {
>     case 1 -> "lunes";
>     case 2 -> "martes";
>     case 3 -> "miércoles";
>     case 4 -> "jueves";
>     case 5 -> "viernes";
>     case 6, 7 -> "fin de semana";
>     default -> throw new IllegalArgumentException("día inválido");
> };
> ```

> [!question]- ¿Qué imprime `System.out.println(7 / 2 + " " + 7 / 2.0);`?
> `3 3.5`: división entera entre `int`, decimal si un operando es `double`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cuál es la estructura mínima de un programa Java?
> ```java
> public class Hola {
>     public static void main(String[] args) {
>         System.out.println("Hola");
>     }
> }
> ```
> El archivo debe llamarse `Hola.java` (igual que la clase pública). Desde Java 21 en adelante existen formas más cortas de `main` (como vista previa en 21 y final en 25).

> [!question]- ¿Diferencia entre `==` y `.equals()` con `String`?
> `==` compara referencias (si es el mismo objeto); `.equals()` compara contenido. En C#, `==` en `string` sí compara contenido porque el operador está sobrecargado; en Java no hay sobrecarga de operadores.

> [!question]- ¿Qué hace `var` en Java?
> Inferencia de tipo para variables locales (desde Java 10), igual que `var` en C#. El tipo sigue siendo estático.

> [!question]- ¿Qué son los tipos primitivos y los wrappers?
> `int`, `long`, `double`, `boolean`, `char`… son primitivos; `Integer`, `Long`… son objetos que los envuelven. Las colecciones solo guardan objetos, así que se usa autoboxing. Cuidado: un `Integer` puede ser `null` y comparar dos `Integer` con `==` compara referencias.

## Práctica

- [[Lab 1 - Primer programa Java con javac, jar y jshell]]

