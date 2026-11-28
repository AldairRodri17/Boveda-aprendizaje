---
proyecto: Conocimiento
tipo: concepto
nivel: intermedio
progreso: pendiente
estado: pendiente
area: IA y Machine Learning
tags: [ia-machine-learning]
contenido: completo
---
# Redes neuronales multicapa

Parte de: [[21-IA-Machine-Learning/04 Redes neuronales y deep learning/_Índice Redes neuronales y deep learning|Redes neuronales y deep learning]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
Una **red neuronal multicapa** o **MLP** (*multilayer perceptron*) apila capas de neuronas: una capa de entrada, una o más **capas ocultas** y una capa de salida. Cada neurona de una capa se conecta con todas las de la siguiente (capas **densas** o *fully connected*). Gracias a las activaciones no lineales entre capas, una MLP puede aprender fronteras curvas y relaciones complejas que una sola [[Neurona artificial y perceptrón|neurona]] no puede, como XOR.

### Cómo funciona
Cada capa hace `h = f(W·x + b)`: una multiplicación de matriz por vector, un sesgo y una activación ([[Funciones de activación]]). La salida de una capa es la entrada de la siguiente; a esto se le llama **forward pass**.

```mermaid
flowchart LR
  E[Entrada: 4 rasgos] --> H1[Oculta: 8 neuronas + ReLU]
  H1 --> H2[Oculta: 8 neuronas + ReLU]
  H2 --> S[Salida: 3 logits]
  S --> P[Softmax: probabilidades]
```

Ideas clave:
- **Sin no linealidad no hay profundidad útil**: dos capas lineales seguidas equivalen a una sola capa lineal (`W₂(W₁x) = (W₂W₁)x`). La activación entre capas es lo que da poder expresivo.
- **Teorema de aproximación universal**: con una capa oculta suficientemente ancha, una MLP puede aproximar cualquier función continua en un dominio acotado. En la práctica, varias capas más angostas suelen aprender mejor que una enorme.
- **Contar parámetros**: una capa densa de `n` entradas a `m` salidas tiene `n·m` pesos más `m` sesgos. Por ejemplo, 10 → 5 tiene `10·5 + 5 = 55` parámetros. Para la red completa sumas capa por capa. Este número te dice cuánta memoria ocupa y qué tan fácil es que memorice.
- **Salida según el problema**: en regresión, una neurona lineal; en clasificación binaria, una neurona con sigmoide; en multiclase, `k` neuronas con [[Softmax]]. La [[Función de pérdida]] se elige en conjunto con esto.

El entrenamiento ajusta todos los pesos con [[Descenso de gradiente y backpropagation]] y un optimizador ([[Optimizadores (SGD y Adam)]]).

### Ejemplo
Clasificación de las tres especies de iris (4 rasgos → 3 clases) con PyTorch. Corre bien en CPU; para datos más grandes, Google Colab te da una GPU sin instalar nada (tu GTX 1660 Ti también sirve para redes pequeñas como esta).

```python
import torch
from torch import nn
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

X, y = load_iris(return_X_y=True)
X_tr, X_te, y_tr, y_te = train_test_split(X, y, test_size=0.2, random_state=42, stratify=y)
esc = StandardScaler().fit(X_tr)                     # escalar con datos de entrenamiento
X_tr, X_te = esc.transform(X_tr), esc.transform(X_te)
X_tr, X_te = torch.tensor(X_tr, dtype=torch.float32), torch.tensor(X_te, dtype=torch.float32)
y_tr, y_te = torch.tensor(y_tr), torch.tensor(y_te)

modelo = nn.Sequential(
    nn.Linear(4, 16), nn.ReLU(),
    nn.Linear(16, 16), nn.ReLU(),
    nn.Linear(16, 3),            # logits; CrossEntropyLoss aplica softmax internamente
)
print("parámetros:", sum(p.numel() for p in modelo.parameters()))   # 4·16+16 + 16·16+16 + 16·3+3 = 403

perdida = nn.CrossEntropyLoss()
opt = torch.optim.Adam(modelo.parameters(), lr=0.01)
for epoca in range(200):
    modelo.train()
    opt.zero_grad()
    loss = perdida(modelo(X_tr), y_tr)
    loss.backward()
    opt.step()

modelo.eval()
with torch.no_grad():
    exactitud = (modelo(X_te).argmax(dim=1) == y_te).float().mean()
print(f"exactitud en prueba: {exactitud:.2%}")
```

### Cuándo usarlo y cuándo no
- **Úsala** con datos vectoriales de tamaño fijo cuando hay relaciones no lineales y suficientes ejemplos, o como "cabeza" final de modelos más grandes (por ejemplo, encima de [[Embeddings]] o de una CNN preentrenada).
- **No es la mejor opción**:
  - Con **datos tabulares** medianos: árboles y gradient boosting suelen ganar con menos ajuste.
  - Con **imágenes**: una MLP ignora la estructura espacial y tiene demasiados parámetros; usa [[Redes convolucionales (CNN)]].
  - Con **secuencias y texto**: usa [[Transformers]] (o [[Redes recurrentes (RNN y LSTM)]] en casos pequeños).
  - Con **pocos datos**: una red grande memoriza en lugar de generalizar.

### Errores comunes
- **Red demasiado grande para pocos datos**: la pérdida de entrenamiento baja casi a cero y la de validación sube; es sobreajuste ([[Overfitting, underfitting y sesgo-varianza]]). Reduce tamaño, junta más datos o usa [[Regularización y dropout]].
- **No escalar las entradas**: rasgos con rangos muy distintos hacen el entrenamiento lento e inestable. Ajusta el escalador solo con el conjunto de entrenamiento.
- **Activación equivocada en la salida**: poner ReLU en la salida de una regresión que puede ser negativa, o softmax antes de `CrossEntropyLoss`.
- **Olvidar las activaciones entre capas**: la red se vuelve un modelo lineal disfrazado.

### Para profundizar
- [CS231n: arquitectura de redes neuronales](https://cs231n.github.io/neural-networks-1/)
- [Tutorial "Build the Neural Network" de PyTorch](https://docs.pytorch.org/tutorials/beginner/basics/buildmodel_tutorial.html)
- [Deep Learning, capítulo 6: Deep Feedforward Networks](https://www.deeplearningbook.org/contents/mlp.html)

## Ejercicios

Resuélvelos en papel o en la herramienta; la solución está plegada.

> [!question]- ¿Cuántos parámetros tiene una MLP 784 → 128 → 10?
> `784·128 + 128 = 100 480`; `128·10 + 10 = 1 290`. Total `101 770`.

> [!question]- ¿Y una 4 → 8 → 3?
> `4·8 + 8 = 40`; `8·3 + 3 = 27`. Total `67`.

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué es una red multicapa (MLP)?
> Capas de neuronas totalmente conectadas: entrada → una o más ocultas (con activación no lineal) → salida.

> [!question]- ¿Qué pasa si agregas muchas capas y neuronas con pocos datos?
> Muchos parámetros y riesgo alto de overfitting; necesitarás regularización, más datos o un modelo más pequeño.

> [!question]- ¿Cuándo una MLP no es la mejor opción?
> Para imágenes (CNN o transformers aprovechan la estructura espacial), secuencias (transformers/RNN) y datos tabulares pequeños (boosting suele ganar).

## Práctica

- [[Lab 3 - Red neuronal pequeña en PyTorch]]

