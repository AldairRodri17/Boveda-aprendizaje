---
proyecto: Conocimiento
tipo: concepto
nivel: basico
progreso: pendiente
estado: pendiente
area: IA para modelado 3D
tags: [ia-modelado-3d]
contenido: completo
---
# Licencia MIT en modelos de IA 3D

Parte de: [[Licencias de modelos de IA 3D]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
La **licencia MIT** es una de las licencias de software más permisivas. Te permite **usar, copiar, modificar, fusionar, publicar, distribuir, sublicenciar y vender** el software sin pagar, con una sola condición: incluir el aviso de copyright y el texto de la licencia en las copias o partes sustanciales que redistribuyas. El software se entrega "tal cual", **sin garantía**. En IA 3D, varios proyectos la aplican también a los **pesos** del modelo.

### Cómo funciona
El texto es corto (unas 170 palabras) y tiene tres partes:
1. **Permiso:** cualquier persona que obtenga una copia puede hacer prácticamente cualquier cosa con ella.
2. **Condición:** el aviso de copyright y este permiso deben incluirse en todas las copias o partes sustanciales.
3. **Exención de garantía:** los autores no responden por daños ni porque el software funcione.

No hay límites de ingresos, de territorio ni de número de usuarios, y **no menciona** los resultados que produce el software: lo que generas con él no queda sujeto a la licencia.

### Modelos de esta área bajo MIT
| Modelo | Qué cubre MIT | Notas |
|---|---|---|
| [[TripoSR]] | Código, modelo preentrenado y demo, según su README | El más usado en tu GTX 1660 Ti |
| [[TRELLIS]] | Modelos y la mayor parte del código | Algunos submódulos tienen su propia licencia |
| TRELLIS.2 | Modelo y código | Algunas dependencias con licencia propia |
| TripoSG | Modelo, según su tarjeta en Hugging Face | Pide más de 8 GB de VRAM |

Revisa siempre el archivo LICENSE y las licencias de las dependencias: un repo MIT puede usar una biblioteca con otra licencia (por ejemplo, un rasterizador con licencia no comercial).

### Ejemplo: tres situaciones
**1. Vendes una impresión hecha con TripoSR.** No tienes que incluir la licencia MIT en la caja ni dar crédito: la condición aplica al redistribuir el **software o los pesos**, no a los objetos generados. Lo que sí importa es que la imagen de entrada sea tuya o libre, y que el diseño no copie un personaje protegido ([[Propiedad y uso comercial de modelos generados por IA]]).

**2. Haces un script de procesamiento por lotes que envuelve TripoSR y lo subes a tu GitHub o a Azure DevOps.** Si incluyes código de TripoSR, conserva su archivo LICENSE con el aviso de copyright original. Tu propio código puede tener la licencia que quieras.

**3. Creas un paquete portable con los pesos de TripoSR y lo compartes.** Estás redistribuyendo los pesos: incluye el aviso de copyright y la licencia MIT. Además revisa las licencias de todo lo que empaquetes (Python, PyTorch, dependencias).

```text
Estructura mínima al redistribuir
mi-paquete/
  LICENSE              <- tu licencia para tu código
  third_party/
    TripoSR-LICENSE    <- texto MIT original con su aviso de copyright
  ...
```

### Cuándo te conviene un modelo MIT
- Quieres **vender** impresiones sin registro ni umbrales.
- Quieres **modificar** el código o integrarlo en una herramienta propia.
- Quieres **compartir** un paquete o flujo con otros.
- Te preocupa que la licencia cambie en el futuro: la versión que descargaste con MIT conserva sus permisos.

### Lo que MIT no te da
- **Garantía:** si el modelo falla o produce algo defectuoso, es tu responsabilidad.
- **Derechos sobre la imagen de entrada** ni sobre personajes o marcas de terceros.
- **Derechos de autor sobre el resultado:** eso lo decide la ley de cada país, no la licencia. En México, una obra creada solo por IA puede no ser protegible.

### Errores comunes
1. **Pensar que MIT obliga a dar crédito en cada impresión.** Solo al redistribuir el software o los pesos.
2. **Borrar el aviso de copyright al copiar código.** Es la única condición; respétala.
3. **Asumir que todo el repo es MIT.** Submódulos y dependencias pueden tener otra licencia.
4. **Confundir MIT con dominio público.** Sigue habiendo un titular de derechos y una condición.

### Para profundizar
- [Texto de la licencia MIT (Open Source Initiative)](https://opensource.org/license/mit)
- [TripoSR en GitHub (licencia MIT)](https://github.com/VAST-AI-Research/TripoSR)
- [TRELLIS en GitHub](https://github.com/microsoft/TRELLIS)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué permite la licencia MIT?
> Usar, copiar, modificar, distribuir y vender el software (y aquí también los pesos del modelo) con la única condición de incluir el aviso de copyright y la licencia al redistribuirlo. Sin garantía.

> [!question]- ¿Qué modelos de esta área son MIT?
> TripoSR (código, modelo y demo, según su [README](https://github.com/VAST-AI-Research/TripoSR)), TRELLIS (modelos y la mayor parte del código; [repo](https://github.com/microsoft/TRELLIS)) y TRELLIS.2 (modelo y código; algunas dependencias tienen su propia licencia).

> [!question]- Si vendes una impresión hecha con TripoSR, ¿debes incluir la licencia MIT?
> No: la condición aplica al redistribuir el **software o los pesos**, no a los objetos que generas. Aun así, la imagen de entrada y lo que representa deben ser tuyos o libres ([[Propiedad y uso comercial de modelos generados por IA]]).

