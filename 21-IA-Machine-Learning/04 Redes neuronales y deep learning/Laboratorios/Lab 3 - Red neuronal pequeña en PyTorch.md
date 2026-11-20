---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: IA y Machine Learning
herramienta: Google Colab (GPU opcional) con PyTorch
tags: [ia-machine-learning, laboratorio]
---
# Lab 3 - Red neuronal pequeña en PyTorch

Parte de: [[21-IA-Machine-Learning/04 Redes neuronales y deep learning/_Índice Redes neuronales y deep learning|Redes neuronales y deep learning]]

**Cubre:** [[PyTorch]] · [[Redes neuronales multicapa]] · [[Función de pérdida]] · [[Descenso de gradiente y backpropagation]] · [[Optimizadores (SGD y Adam)]] · [[Regularización y dropout]] · [[Softmax]] · [[GPUs y por qué importan]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir, entrenar y evaluar una MLP en PyTorch para clasificar los dígitos de `load_digits` (imágenes de 8×8) y entender cada paso del ciclo de entrenamiento.

## Pasos

1. En Colab, revisa si hay GPU (`torch.cuda.is_available()`) y define `device`.
2. Carga `sklearn.datasets.load_digits()`, divide 80/20 estratificado y escala con `StandardScaler` ajustado en entrenamiento.
3. Convierte a tensores (`float32` para X, `long` para y) y crea `DataLoader` con lotes de 64.
4. Define una MLP 64 → 128 → 64 → 10 con ReLU y `Dropout(0.2)`. Calcula a mano el número de parámetros y compáralo con `sum(p.numel() for p in model.parameters())`.
5. Entrena 30 épocas con `nn.CrossEntropyLoss` y Adam (lr 1e-3), registrando la pérdida de entrenamiento y la exactitud de prueba por época.
6. Grafica pérdida y exactitud por época.
7. Repite con SGD (lr 0.01) y compara la velocidad de convergencia.
8. Guarda el modelo con `torch.save(model.state_dict(), 'mlp.pt')`, vuelve a cargarlo y predice un ejemplo.

## Criterios de terminado

- [ ] El conteo de parámetros calculado a mano coincide con el que reporta PyTorch (17 226 para 64 → 128 → 64 → 10).
- [ ] La exactitud en prueba con Adam supera 90 %.
- [ ] El bucle usa `zero_grad`, `backward`, `step`, y la evaluación usa `model.eval()` y `torch.no_grad()`.
- [ ] Hay una gráfica de pérdida por época con Adam y con SGD.
- [ ] El modelo recargado da la misma predicción que el original.

## Solución

> [!success]- Conteo de parámetros
> `64·128 + 128 = 8 320`; `128·64 + 64 = 8 256`; `64·10 + 10 = 650`. Total **17 226**. Dropout y ReLU no tienen parámetros.

> [!success]- Código completo
> ```python
> import torch, torch.nn as nn
> from torch.utils.data import TensorDataset, DataLoader
> from sklearn.datasets import load_digits
> from sklearn.model_selection import train_test_split
> from sklearn.preprocessing import StandardScaler
>
> device = 'cuda' if torch.cuda.is_available() else 'cpu'
> X, y = load_digits(return_X_y=True)
> X_tr, X_te, y_tr, y_te = train_test_split(X, y, test_size=0.2, stratify=y, random_state=0)
> sc = StandardScaler().fit(X_tr)
> t = lambda a, dt: torch.tensor(a, dtype=dt)
> tr = DataLoader(TensorDataset(t(sc.transform(X_tr), torch.float32), t(y_tr, torch.long)), batch_size=64, shuffle=True)
> Xte, yte = t(sc.transform(X_te), torch.float32).to(device), t(y_te, torch.long).to(device)
>
> def crear():
>     return nn.Sequential(nn.Linear(64,128), nn.ReLU(), nn.Dropout(0.2),
>                          nn.Linear(128,64), nn.ReLU(), nn.Dropout(0.2),
>                          nn.Linear(64,10)).to(device)
>
> def entrenar(model, opt, epocas=30):
>     loss_fn, hist = nn.CrossEntropyLoss(), []
>     for ep in range(epocas):
>         model.train(); total = 0
>         for xb, yb in tr:
>             xb, yb = xb.to(device), yb.to(device)
>             opt.zero_grad()
>             loss = loss_fn(model(xb), yb)   # logits crudos, sin softmax
>             loss.backward(); opt.step()
>             total += loss.item() * len(xb)
>         model.eval()
>         with torch.no_grad():
>             acc = (model(Xte).argmax(1) == yte).float().mean().item()
>         hist.append((total / len(tr.dataset), acc))
>     return hist
>
> m = crear(); print(sum(p.numel() for p in m.parameters()))
> h_adam = entrenar(m, torch.optim.Adam(m.parameters(), lr=1e-3))
> m2 = crear(); h_sgd = entrenar(m2, torch.optim.SGD(m2.parameters(), lr=0.01))
> print(h_adam[-1], h_sgd[-1])
> ```

> [!success]- Gráfica y guardado
> ```python
> import matplotlib.pyplot as plt
> plt.plot([h[0] for h in h_adam], label='Adam'); plt.plot([h[0] for h in h_sgd], label='SGD')
> plt.xlabel('época'); plt.ylabel('pérdida'); plt.legend(); plt.show()
>
> torch.save(m.state_dict(), 'mlp.pt')
> m3 = crear(); m3.load_state_dict(torch.load('mlp.pt')); m3.eval(); m.eval()
> with torch.no_grad():
>     print(m(Xte[:1]).argmax(1), m3(Xte[:1]).argmax(1))
> ```

## Notas personales

_Qué me costó, errores que cometí, dudas._
