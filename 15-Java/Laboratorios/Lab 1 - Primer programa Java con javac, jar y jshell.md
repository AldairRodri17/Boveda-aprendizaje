---
proyecto: Conocimiento
tipo: laboratorio
nivel: basico
progreso: pendiente
estado: borrador
area: Java
herramienta: JDK (Temurin o Microsoft Build of OpenJDK), VS Code con Extension Pack for Java
tags: [java, laboratorio]
---
# Lab 1 - Primer programa Java con javac, jar y jshell

Parte de: [[15-Java/_Índice Java|Java]]

**Cubre:** [[Java - JVM, JDK y JRE]] · [[Java - sintaxis básica]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo
Instalar un JDK LTS, compilar y ejecutar un programa a mano (sin IDE ni Maven), empaquetarlo en un `.jar` ejecutable y probar expresiones en `jshell`.

## Pasos
1. Instala un JDK LTS (por ejemplo con `winget`) y verifica `java -version` y `javac -version`.
2. Crea `src/mx/lab/Hola.java` con un `main` que reciba un nombre por argumento y salude; si no hay argumento, que salude a "mundo".
3. Compila con `javac -d out` y ejecuta con `java -cp out`.
4. Abre la carpeta `out` y localiza el `.class`. Desensambla con `javap -c` para ver el bytecode.
5. Crea un `.jar` ejecutable con punto de entrada y ejecútalo con `java -jar`.
6. Abre `jshell` y prueba: comparación de `String` con `==` y `equals`, división entera y un `switch` moderno.
7. Escribe en tus notas personales tres equivalencias con .NET que notaste.

## Criterios de terminado
- [ ] `java -version` muestra una versión LTS.
- [ ] `java -cp out mx.lab.Hola Ana` imprime `Hola, Ana`.
- [ ] `java -jar hola.jar` imprime `Hola, mundo`.
- [ ] Viste el bytecode con `javap -c` y ubicaste la llamada a `println`.
- [ ] En `jshell` comprobaste que `new String("a") == "a"` es `false`.

## Solución

> [!success]- Instalación en Windows
> ```powershell
> winget install EclipseAdoptium.Temurin.21.JDK
> # o: winget install Microsoft.OpenJDK.21
> java -version
> ```
> Los ids de `winget` pueden cambiar; búscalos con `winget search temurin` (verificar en la página oficial).

> [!success]- Código
> ```java
> package mx.lab;
>
> public class Hola {
>     public static void main(String[] args) {
>         String nombre = args.length > 0 ? args[0] : "mundo";
>         System.out.println("Hola, " + nombre);
>     }
> }
> ```

> [!success]- Compilar, ejecutar y empaquetar
> ```powershell
> javac -d out src/mx/lab/Hola.java
> java -cp out mx.lab.Hola Ana
> javap -c -cp out mx.lab.Hola
> jar --create --file hola.jar --main-class mx.lab.Hola -C out .
> java -jar hola.jar
> ```

> [!success]- Equivalencias con .NET
> - `javac` ≈ compilador de C#; `.class` ≈ IL dentro de un ensamblado.
> - `java -jar` ≈ `dotnet app.dll`.
> - `jshell` ≈ `dotnet fsi` o C# Interactive (`csi`).

## Notas personales

_Qué me costó, errores que cometí, dudas._
