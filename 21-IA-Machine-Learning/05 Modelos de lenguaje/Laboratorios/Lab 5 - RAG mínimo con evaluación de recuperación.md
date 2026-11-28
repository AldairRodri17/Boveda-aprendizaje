---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: IA y Machine Learning
herramienta: Python con sentence-transformers y un LLM local con Ollama (o cualquier API con capa gratuita)
tags: [ia-machine-learning, laboratorio]
contenido: completo
---
# Lab 5 - RAG mínimo con evaluación de recuperación

Parte de: [[21-IA-Machine-Learning/05 Modelos de lenguaje/_Índice Modelos de lenguaje|Modelos de lenguaje]]

**Cubre:** [[RAG (generación aumentada con recuperación)]] · [[Chunking y reranking]] · [[Prompting efectivo]] · [[Alucinaciones y cómo reducirlas]] · [[Cómo funciona un LLM (tokens, contexto y predicción)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir un RAG de principio a fin sobre 5–10 de tus propias notas de la bóveda: chunking, embeddings, recuperación top-k, prompt con citas y una medición sencilla de si la recuperación encuentra el fragmento correcto.

## Pasos

1. Instala Ollama en tu PC y descarga un modelo pequeño de chat (elige uno de su catálogo que quepa en tu memoria). Comprueba que responde en `http://localhost:11434`. Alternativa: cualquier API de LLM con capa gratuita.
2. Copia 5–10 notas `.md` de tu bóveda a una carpeta `docs/` (no uses notas con datos sensibles).
3. Divide cada nota en chunks por párrafos/títulos de ~800 caracteres con 100 de solapamiento; guarda el nombre del archivo en cada chunk.
4. Genera embeddings normalizados de los chunks con sentence-transformers.
5. Implementa `recuperar(pregunta, k=4)`.
6. Arma un prompt que delimite el contexto con etiquetas, pida responder **solo** con esa información, citar el archivo y decir "No lo encuentro en las notas" si no está.
7. Escribe 8 preguntas cuya respuesta sepas en qué nota está, y 2 cuya respuesta no esté en ninguna.
8. Mide hit rate@4 (¿el archivo correcto aparece entre los recuperados?) y revisa a mano si el modelo dijo "No lo encuentro" en las 2 preguntas sin respuesta.

## Criterios de terminado

- [ ] Cada chunk guarda su archivo de origen y la respuesta incluye la cita del archivo.
- [ ] Se reporta hit rate@4 sobre las 8 preguntas.
- [ ] En al menos 1 de las 2 preguntas sin respuesta el modelo contesta que no lo encuentra.
- [ ] Si una respuesta es incorrecta, anotaste si falló la recuperación o la generación.

## Solución

> [!success]- Ingesta y recuperación
> ```python
> import pathlib, re, numpy as np
> from sentence_transformers import SentenceTransformer
>
> def trocear(texto, tam=800, solape=100):
>     partes, i = [], 0
>     while i < len(texto):
>         partes.append(texto[i:i+tam]); i += tam - solape
>     return partes
>
> chunks = []
> for p in pathlib.Path('docs').glob('*.md'):
>     t = re.sub(r'^---.*?---', '', p.read_text(encoding='utf-8'), flags=re.S)
>     chunks += [{'archivo': p.name, 'texto': c} for c in trocear(t)]
> modelo = SentenceTransformer('paraphrase-multilingual-MiniLM-L12-v2')
> E = modelo.encode([c['texto'] for c in chunks], normalize_embeddings=True)
>
> def recuperar(pregunta, k=4):
>     q = modelo.encode([pregunta], normalize_embeddings=True)[0]
>     return [chunks[i] for i in np.argsort(-(E @ q))[:k]]
> ```
> Mejora opcional: cortar primero por títulos `#` y luego por tamaño.

> [!success]- Generación con Ollama
> ```python
> import requests
> MODELO = 'el-modelo-que-descargaste'
>
> def responder(pregunta):
>     ctx = recuperar(pregunta)
>     contexto = '\n'.join(f"<doc archivo='{c['archivo']}'>\n{c['texto']}\n</doc>" for c in ctx)
>     prompt = ("Responde usando SOLO la información de <contexto>. Cita el archivo entre corchetes. "
>               "Si no está, responde exactamente: No lo encuentro en las notas.\n"
>               f"<contexto>\n{contexto}\n</contexto>\nPregunta: {pregunta}")
>     r = requests.post('http://localhost:11434/api/chat', json={
>         'model': MODELO, 'stream': False, 'options': {'temperature': 0},
>         'messages': [{'role': 'user', 'content': prompt}]})
>     return r.json()['message']['content'], [c['archivo'] for c in ctx]
> ```

> [!success]- Evaluación
> ```python
> pruebas = [('¿Qué es una VLAN nativa?', 'VLAN nativa.md'), ...]  # 8 con archivo esperado
> aciertos = sum(esperado in [c['archivo'] for c in recuperar(p)] for p, esperado in pruebas)
> print('hit rate@4 =', aciertos / len(pruebas))
> ```
> Si el archivo correcto no se recupera: revisa tamaño de chunk, k o el modelo de embeddings. Si se recupera pero la respuesta es mala: ajusta el prompt o el modelo de chat.

## Notas personales

_Qué me costó, errores que cometí, dudas._
