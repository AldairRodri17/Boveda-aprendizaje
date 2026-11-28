---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: IA y Machine Learning
tags: [ia-machine-learning]
contenido: completo
---
# Python - básico

Parte de: [[21-IA-Machine-Learning/02 Python para datos/_Índice Python para datos|Python para datos]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Python es el lenguaje dominante en ciencia de datos y ML, no por ser el más rápido, sino porque es fácil de leer y tiene el ecosistema más grande (NumPy, pandas, scikit-learn, PyTorch). Viniendo de C#, lo que más cambia es que el **tipado es dinámico** (las variables no declaran tipo, aunque puedes anotar con *type hints*), los bloques se marcan con **indentación** en lugar de llaves, y hay mucha sintaxis compacta para trabajar con colecciones. Con lo de esta nota puedes leer el 90 % del código de notebooks de ML.

### Cómo funciona
**Las cuatro colecciones básicas** (piénsalas como sus equivalentes .NET):

| Python | Ejemplo | Mutable | Equivalente aproximado en C# |
|---|---|---|---|
| `list` | `[1, 2, 3]` | sí, ordenada | `List<T>` |
| `tuple` | `(1, 2)` | no, ordenada | `ValueTuple` / record inmutable |
| `dict` | `{"a": 1}` | sí, por clave | `Dictionary<K,V>` |
| `set` | `{1, 2}` | sí, sin duplicados | `HashSet<T>` |

Las tuplas sirven como claves de diccionario y para devolver varios valores (`return x, y`). Los conjuntos son ideales para "¿ya vi este valor?" y operaciones como intersección (`a & b`).

**Slicing.** `secuencia[inicio:fin:paso]`, con `fin` excluido. `s[::-1]` invierte; `s[-1]` es el último elemento. Funciona igual en listas, cadenas y, con más dimensiones, en arreglos de [[NumPy]].

**Comprehensions.** Una forma declarativa de construir colecciones, parecida a un `Select`/`Where` de LINQ en una línea: `[x**2 for x in datos if x % 2 == 0]`. También existen para diccionarios (`{k: v for ...}`) y conjuntos. Si la comprehension crece mucho, mejor usa un `for` normal.

**Funciones.** `def nombre(param, opcional=valor):`. Puedes pasar argumentos por nombre (`f(x=1)`), recibir variables con `*args` y `**kwargs`, y documentar tipos con `def f(x: int) -> float:`. Los type hints no se validan en ejecución; los revisa una herramienta como mypy o el editor.

**Trampa clásica: argumento por defecto mutable.** `def f(x, lista=[])` crea **una sola lista** cuando se define la función, y se reutiliza en cada llamada. Usa `lista=None` y créala adentro.

**Otras piezas que verás seguido:** `enumerate` (índice y valor), `zip` (recorrer en paralelo), f-strings (`f"{valor:.2f}"`), `with open(...)` (equivale a `using`), `collections.Counter` (conteos) y `dataclasses` (parecido a un `record`).

### Ejemplo

```python
from collections import Counter
from dataclasses import dataclass

# Cuadrados de los pares del 1 al 10 (comprehension)
cuadrados = [x**2 for x in range(1, 11) if x % 2 == 0]
print(cuadrados)                       # [4, 16, 36, 64, 100]

# Contar palabras: a mano con dict y con Counter
texto = "a b a c b a"
conteo = {}
for palabra in texto.split():
    conteo[palabra] = conteo.get(palabra, 0) + 1
print(conteo, Counter(texto.split()).most_common(1))   # {'a': 3, 'b': 2, 'c': 1} [('a', 3)]

# Slicing
print("datos"[::-1], [10, 20, 30, 40][1:3])            # sotad [20, 30]

# El bug del argumento mutable y su arreglo
def agrega_mal(x, lista=[]):
    lista.append(x); return lista

def agrega_bien(x, lista=None):
    lista = [] if lista is None else lista
    lista.append(x); return lista

print(agrega_mal(1), agrega_mal(2))     # [1, 2] [1, 2]  <- la misma lista compartida
print(agrega_bien(1), agrega_bien(2))   # [1] [2]

@dataclass
class Venta:                            # similar a un record de C#
    ciudad: str
    monto: float

ventas = [Venta("CDMX", 1200), Venta("GDL", 800), Venta("CDMX", 300)]
por_ciudad = {}
for v in ventas:
    por_ciudad[v.ciudad] = por_ciudad.get(v.ciudad, 0) + v.monto
print(por_ciudad)                       # {'CDMX': 1500, 'GDL': 800}
```

Para datos tabulares reales no harías estos ciclos: usarías [[pandas]] (`groupby`). Pero entender listas, diccionarios y comprehensions es lo que te deja leer y depurar ese código.

### Cuándo usarlo y cuándo no
Usa Python puro para lógica, scripts, orquestación y limpieza ligera. Para cálculo numérico sobre muchos datos, los ciclos `for` de Python son lentos: pasa a operaciones vectorizadas de NumPy o pandas. Si tu meta es solo **consumir** un modelo desde una API .NET, no necesitas Python en producción: puedes entrenar con [[ML.NET]] o exportar el modelo a ONNX y servirlo con [[ONNX Runtime]] en C#.

### Errores comunes
- **Argumentos por defecto mutables** (`[]`, `{}`): se comparten entre llamadas.
- **Mezclar tabs y espacios** o indentar mal: la indentación es sintaxis. Usa 4 espacios y deja que el editor formatee.
- **Copiar por referencia sin querer.** `b = a` en una lista no la copia; usa `a.copy()` o `list(a)`.
- **Comparar con `is` en vez de `==`.** `is` compara identidad de objeto; para valores usa `==` (excepto `is None`).
- **Usar `for` con índices** (`for i in range(len(x))`) cuando `enumerate` o `zip` es más claro.

### Para profundizar
- [Tutorial oficial de Python (en español)](https://docs.python.org/es/3/tutorial/)
- [Python: estructuras de datos](https://docs.python.org/es/3/tutorial/datastructures.html)
- [PEP 8: guía de estilo](https://peps.python.org/pep-0008/)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- Con una comprehension, obtén los cuadrados de los pares del 1 al 10.
> ```python
> [x**2 for x in range(1, 11) if x % 2 == 0]
> # [4, 16, 36, 64, 100]
> ```

> [!question]- Cuenta cuántas veces aparece cada palabra en `"a b a c b a"`.
> ```python
> from collections import Counter
> Counter("a b a c b a".split())
> # Counter({'a': 3, 'b': 2, 'c': 1})
> ```

> [!question]- Invierte la cadena `"datos"` con slicing.
> `"datos"[::-1]` → `'sotad'`

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué diferencia hay entre lista, tupla, diccionario y conjunto?
> Lista `[]`: ordenada y mutable. Tupla `()`: ordenada e inmutable. Diccionario `{k: v}`: pares clave-valor. Conjunto `{}`/`set()`: elementos únicos sin orden.

> [!question]- ¿Qué es una list comprehension?
> Una forma compacta de construir listas: `[x*x for x in datos if x > 0]`. Más legible que un `for` con `append` para transformaciones simples.

> [!question]- Error común: `def f(x, lista=[])`. ¿Qué pasa?
> El valor por defecto se crea una sola vez; las llamadas comparten la misma lista y se acumulan datos. Usa `lista=None` y dentro `lista = lista or []`.

