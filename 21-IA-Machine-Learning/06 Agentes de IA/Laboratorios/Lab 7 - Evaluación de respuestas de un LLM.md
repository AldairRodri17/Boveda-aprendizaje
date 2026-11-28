---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: IA y Machine Learning
herramienta: Python con Ollama (o API con capa gratuita) y pandas
tags: [ia-machine-learning, laboratorio]
contenido: completo
---
# Lab 7 - Evaluación de respuestas de un LLM

Parte de: [[21-IA-Machine-Learning/06 Agentes de IA/_Índice Agentes de IA|Agentes de IA]]

**Cubre:** [[Evaluación de respuestas de LLMs]] · [[Evaluación de agentes]] · [[Salidas estructuradas (JSON)]] · [[Parámetros de generación (temperatura y top-p)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Crear un conjunto de evaluación pequeño y medir automáticamente dos variantes de prompt con verificaciones exactas y con un LLM como juez, repitiendo corridas para ver la variabilidad.

## Pasos

1. Elige una tarea con respuesta verificable: extraer `{nombre, monto, fecha}` en JSON de 15 mensajes cortos que escribas tú (con la respuesta esperada).
2. Escribe dos prompts: A (instrucción breve) y B (instrucción + esquema + 2 ejemplos).
3. Para cada mensaje y prompt, llama al modelo con temperatura 0 y guarda la salida.
4. Métrica 1: porcentaje de JSON válido. Métrica 2: exactitud por campo comparando con lo esperado.
5. Agrega un juez LLM con rúbrica 1–5 para una pregunta abierta (resumen de un mensaje) y pide que devuelva JSON `{puntaje, razon}`.
6. Revisa a mano 5 puntajes del juez y anota si estás de acuerdo.
7. Repite el prompt B 3 veces con temperatura 0.8 y mide cuánto varía la exactitud.
8. Resume en una tabla de pandas: prompt, % JSON válido, exactitud por campo.

## Criterios de terminado

- [ ] Existe un archivo `eval.jsonl` con 15 casos y su respuesta esperada.
- [ ] La tabla compara A y B en % JSON válido y exactitud por campo.
- [ ] Se reportan media y rango de 3 corridas con temperatura 0.8.
- [ ] Hay una nota sobre cuántas veces coincidiste con el juez en las 5 revisiones.

## Solución

> [!success]- Evaluación exacta
> ```python
> import json, pandas as pd
> casos = [json.loads(l) for l in open('eval.jsonl', encoding='utf-8')]
> # cada caso: {'texto': ..., 'esperado': {'nombre': ..., 'monto': ..., 'fecha': ...}}
>
> def evaluar(prompt, temp=0):
>     validos, aciertos = 0, {'nombre': 0, 'monto': 0, 'fecha': 0}
>     for c in casos:
>         salida = llamar_llm(prompt.format(texto=c['texto']), temp)  # tu función con Ollama/API
>         try:
>             d = json.loads(salida); validos += 1
>         except json.JSONDecodeError:
>             continue
>         for k in aciertos:
>             aciertos[k] += str(d.get(k, '')).strip() == str(c['esperado'][k])
>     n = len(casos)
>     return {'json_valido': validos / n, **{k: v / n for k, v in aciertos.items()}}
>
> pd.DataFrame([{'prompt': 'A', **evaluar(PROMPT_A)}, {'prompt': 'B', **evaluar(PROMPT_B)}])
> ```

> [!success]- Juez con rúbrica
> ```text
> Evalúa el RESUMEN frente al MENSAJE. Rúbrica:
> 5 = correcto, completo y breve; 3 = correcto pero omite un dato clave; 1 = contiene datos falsos.
> Responde solo JSON: {"puntaje": 1-5, "razon": "..."}
> ```
> Calibra: si discrepas del juez en más de 1 de 5 casos, afina la rúbrica con ejemplos de cada puntaje.

## Notas personales

_Qué me costó, errores que cometí, dudas._
