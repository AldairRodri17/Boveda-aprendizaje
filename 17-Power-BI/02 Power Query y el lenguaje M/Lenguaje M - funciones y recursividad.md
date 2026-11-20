---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: Power BI
tags: [power-bi]
---
# Lenguaje M - funciones y recursividad

Parte de: [[17-Power-BI/02 Power Query y el lenguaje M/_Índice Power Query y el lenguaje M|Power Query y el lenguaje M]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
En M las **funciones son valores**: se guardan en variables, se pasan como argumentos y se devuelven desde otras funciones, igual que las lambdas de C#. Esto permite escribir transformaciones reutilizables y aplicar lógica a listas y tablas con funciones de orden superior (`List.Transform`, `Table.AddColumn`, `List.Accumulate`). La **recursividad** también es posible, aunque en Power Query casi siempre hay una opción mejor.

### Cómo funciona
**Definir una función**: lista de parámetros entre paréntesis, `=>` y el cuerpo.

```m
(a as number, b as number) as number => a + b
```

- Los tipos (`as number`) son opcionales pero recomendables.
- `optional` marca parámetros opcionales: `(x, optional y) => ...`.
- `each expr` es atajo de `(_) => expr`; `_` es el parámetro.
- Se invoca con paréntesis: `Suma(2, 3)`.

**Recursividad**: una función se llama a sí misma con el **operador de alcance `@`** delante de su nombre, que permite referenciar la variable que se está definiendo:

```m
let
    Factorial = (n as number) as number =>
        if n <= 1 then 1 else n * @Factorial(n - 1)
in
    Factorial(5)   // 120
```

**¿Por qué evitar la recursión en Power Query?** Porque M no optimiza llamadas en cola: cada llamada ocupa pila y memoria, en listas largas puede desbordarse (*stack overflow*) o ser muy lenta, y es difícil de depurar en la interfaz. Las funciones de la biblioteca hacen lo mismo de forma iterativa y eficiente:

- **`List.Generate(inicial, condición, siguiente, selector)`**: genera una lista mientras se cumpla una condición, ideal para **paginación** de APIs y series.
- **`List.Accumulate(lista, semilla, (estado, actual) => ...)`**: recorre una lista acumulando un estado (el *fold* o `Aggregate` de LINQ).
- **`List.Transform`**: aplica una función a cada elemento (`Select` de LINQ).

### Ejemplo
Suma de cuadrados con `List.Accumulate`:

```m
List.Accumulate({1, 2, 3, 4}, 0, (estado, x) => estado + x * x)   // 30
```

Mayúsculas con `List.Transform`:

```m
List.Transform({"a", "b"}, Text.Upper)   // {"A", "B"}  (la función se pasa como valor)
```

Aplicar una lista de reemplazos a un texto (encadenar transformaciones con `Accumulate`):

```m
let
    Reemplazos = {{"Á", "A"}, {"É", "E"}, {"Í", "I"}, {"Ó", "O"}, {"Ú", "U"}},
    QuitarAcentos = (t as text) as text =>
        List.Accumulate(Reemplazos, Text.Upper(t), (s, par) => Text.Replace(s, par{0}, par{1})),
    Resultado = QuitarAcentos("Querétaro")   // "QUERETARO"
in
    Resultado
```

Serie de fechas de cierre mensual con `List.Generate`, sin recursión:

```m
List.Generate(
    () => #date(2025, 1, 31),
    each _ <= #date(2025, 12, 31),
    each Date.EndOfMonth(Date.AddDays(_, 1)))
// {31-ene, 28-feb, 31-mar, ..., 31-dic}
```

Función que devuelve función (*closure*), útil para fabricar validadores:

```m
let
    MayorQue = (limite as number) => (x as number) => x > limite,
    Mayor1000 = MayorQue(1000),
    Grandes = List.Select({500, 1500, 3000}, Mayor1000)   // {1500, 3000}
in
    Grandes
```

### Cuándo usarlo y cuándo no
- **Funciones** siempre que repitas lógica; expórtalas como consultas de función (ver [[Power Query - funciones personalizadas]]).
- **`List.Generate`** para paginar y para cualquier "bucle mientras".
- **`List.Accumulate`** para reducir una lista a un valor o aplicar transformaciones encadenadas.
- **Recursión con `@`** solo para estructuras jerárquicas pequeñas o ejercicios; no con miles de niveles.
- **Pasa funciones como valores** (`List.Transform(lista, Text.Upper)`) en lugar de envolverlas en `each Text.Upper(_)` cuando no necesitas más lógica: es lo mismo, pero más corto y legible.

### Errores comunes
- **Olvidar `@`** al llamarse a sí misma: error de nombre no reconocido.
- **Recursión profunda** en datos reales: desbordamiento de pila.
- **Confundir `each` anidados**: dentro de un `each` dentro de otro, `_` se refiere al más interno; usa lambdas con nombre `(fila) =>` para claridad.
- **`List.Accumulate` con tablas grandes** como estado: cada paso copia la tabla; es lento.

### Para profundizar
- [Funciones en la especificación de M](https://learn.microsoft.com/es-mx/powerquery-m/m-spec-functions)
- [List.Generate](https://learn.microsoft.com/es-mx/powerquery-m/list-generate)
- [List.Accumulate](https://learn.microsoft.com/es-mx/powerquery-m/list-accumulate)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Escribe una función recursiva para el factorial y evalúa 5.
> ```m
> let
>     Factorial = (n as number) as number => if n <= 1 then 1 else n * @Factorial(n - 1)
> in
>     Factorial(5)   // 120
> ```

> [!question]- Con `List.Accumulate`, suma los cuadrados de `{1, 2, 3, 4}`.
> ```m
> List.Accumulate({1, 2, 3, 4}, 0, (estado, x) => estado + x * x)   // 30
> ```

> [!question]- Con `List.Transform`, convierte `{"a", "b"}` a mayúsculas.
> ```m
> List.Transform({"a", "b"}, Text.Upper)   // {"A", "B"}
> ```

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Cómo se define una función en M?
> `(x as number, optional y as number) as number => x + (if y = null then 0 else y)`. Las funciones son valores: se asignan a variables y se pasan a otras funciones.

> [!question]- ¿Qué operador permite recursividad?
> `@`, para referirse a la función dentro de su propia definición: `@Factorial(n - 1)`.

> [!question]- ¿Por qué en Power Query se prefiere `List.Generate` o `List.Accumulate` a la recursión?
> La recursión profunda puede ser lenta o desbordar la pila; `List.Generate`/`List.Accumulate` iteran de forma controlada.

