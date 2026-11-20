---
proyecto: Conocimiento
tipo: laboratorio
nivel: avanzado
progreso: pendiente
estado: borrador
area: IA y Machine Learning
herramienta: Python, Ollama (modelo con soporte de herramientas) y el SDK oficial de MCP para Python
tags: [ia-machine-learning, laboratorio]
---
# Lab 6 - Agente con tool calling y un servidor MCP sencillo

Parte de: [[21-IA-Machine-Learning/06 Agentes de IA/_Índice Agentes de IA|Agentes de IA]]

**Cubre:** [[Tool calling (llamadas a herramientas)]] · [[Qué es un agente de IA]] · [[El ciclo de un agente (razonar, actuar, observar)]] · [[Herramientas y MCP (Model Context Protocol)]] · [[Guardrails y límites de un agente]] · [[Humano en el ciclo (aprobaciones)]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Programar desde cero un bucle de agente con tool calling (sin framework) y exponer las mismas herramientas en un servidor MCP local que puedas probar con el MCP Inspector.

## Pasos

1. Con Ollama, descarga un modelo que soporte herramientas (*tools*) según su catálogo.
2. Define dos herramientas en Python: `sumar(a, b)` y `buscar_nota(titulo)` que lea un `.md` de una carpeta de prueba **de solo lectura** (copia, no tu bóveda real).
3. Descríbelas con JSON Schema y envíalas en `tools` a `/api/chat`.
4. Implementa el bucle: si la respuesta trae `tool_calls`, valida los argumentos, ejecuta la función, agrega el resultado como mensaje `tool` y vuelve a llamar; termina cuando no haya más llamadas o tras 5 iteraciones.
5. Agrega una tercera herramienta `borrar_nota(titulo)` que **no** borre: pide confirmación por consola (`input`) y solo imprime lo que haría.
6. Instala el SDK de MCP (`pip install mcp`) y crea un servidor que exponga `sumar` y `buscar_nota` como tools.
7. Pruébalo con el MCP Inspector (`npx @modelcontextprotocol/inspector`) apuntando a tu servidor por stdio y ejecuta cada herramienta.
8. Documenta en la sección Notas personales qué pasó cuando le pediste al agente algo que requiere dos herramientas seguidas.

## Criterios de terminado

- [ ] El agente responde "¿cuánto es 17.5 + 24.5?" llamando a `sumar` y contesta 42.
- [ ] El bucle se detiene a las 5 iteraciones como máximo.
- [ ] `buscar_nota` rechaza rutas con `..` o fuera de la carpeta de prueba.
- [ ] `borrar_nota` nunca se ejecuta sin escribir `si` en la consola.
- [ ] El MCP Inspector lista las dos herramientas del servidor y ejecuta `sumar` correctamente.

## Solución

> [!success]- Bucle del agente (Ollama)
> ```python
> import json, pathlib, requests
> BASE = pathlib.Path('notas_prueba').resolve()
> MODELO = 'modelo-con-tools'
>
> def sumar(a: float, b: float) -> float:
>     return a + b
>
> def buscar_nota(titulo: str) -> str:
>     p = (BASE / f'{titulo}.md').resolve()
>     if BASE not in p.parents:            # evita ../ fuera de la carpeta
>         return 'Error: ruta no permitida'
>     return p.read_text(encoding='utf-8')[:2000] if p.exists() else 'No existe'
>
> def borrar_nota(titulo: str) -> str:
>     if input(f'¿Borrar {titulo}? (si/no) ') != 'si':
>         return 'El usuario rechazó la acción'
>     return f'(simulado) se borraría {titulo}'
>
> FUN = {'sumar': sumar, 'buscar_nota': buscar_nota, 'borrar_nota': borrar_nota}
> def tool(n, d, props):
>     return {'type': 'function', 'function': {'name': n, 'description': d,
>             'parameters': {'type': 'object', 'properties': props, 'required': list(props)}}}
> TOOLS = [tool('sumar', 'Suma dos números', {'a': {'type': 'number'}, 'b': {'type': 'number'}}),
>          tool('buscar_nota', 'Lee una nota por título', {'titulo': {'type': 'string'}}),
>          tool('borrar_nota', 'Borra una nota (requiere aprobación)', {'titulo': {'type': 'string'}})]
>
> def agente(pregunta, max_pasos=5):
>     msgs = [{'role': 'user', 'content': pregunta}]
>     for _ in range(max_pasos):
>         r = requests.post('http://localhost:11434/api/chat', json={'model': MODELO, 'messages': msgs,
>                           'tools': TOOLS, 'stream': False}).json()['message']
>         msgs.append(r)
>         if not r.get('tool_calls'):
>             return r['content']
>         for tc in r['tool_calls']:
>             f, args = tc['function']['name'], tc['function']['arguments']
>             try:
>                 res = FUN[f](**args) if f in FUN else 'Herramienta desconocida'
>             except Exception as e:
>                 res = f'Error: {e}'           # el error vuelve como observación
>             msgs.append({'role': 'tool', 'content': str(res)})
>     return 'Límite de pasos alcanzado'
>
> print(agente('¿Cuánto es 17.5 + 24.5?'))
> ```
> El formato exacto de `tool_calls` puede variar entre versiones de Ollama: (Pendiente de confirmar en la tuya; imprime `r` para verlo).

> [!success]- Servidor MCP
> ```python
> # servidor_mcp.py
> from mcp.server.fastmcp import FastMCP
> import pathlib
> BASE = pathlib.Path('notas_prueba').resolve()
> mcp = FastMCP('notas-demo')
>
> @mcp.tool()
> def sumar(a: float, b: float) -> float:
>     """Suma dos números."""
>     return a + b
>
> @mcp.tool()
> def buscar_nota(titulo: str) -> str:
>     """Lee una nota de la carpeta de prueba por su título."""
>     p = (BASE / f'{titulo}.md').resolve()
>     if BASE not in p.parents:
>         return 'Error: ruta no permitida'
>     return p.read_text(encoding='utf-8')[:2000] if p.exists() else 'No existe'
>
> if __name__ == '__main__':
>     mcp.run()   # stdio por defecto
> ```
> Prueba: `npx @modelcontextprotocol/inspector python servidor_mcp.py`. La API del SDK cambia entre versiones (Pendiente de confirmar la de la versión que instales; revisa el README del SDK).

## Notas personales

_Qué me costó, errores que cometí, dudas._
