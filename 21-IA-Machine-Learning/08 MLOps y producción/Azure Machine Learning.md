---
proyecto: Conocimiento
tipo: concepto
nivel: avanzado
progreso: pendiente
estado: pendiente
area: IA y Machine Learning
tags: [ia-machine-learning]
---
# Azure Machine Learning

Parte de: [[21-IA-Machine-Learning/08 MLOps y producción/_Índice MLOps y producción|MLOps y producción]]

> [!info] Cómo avanzar
> `progreso`: **pendiente** → **viendo** (estudiando) → **visto** (contestaste la autoevaluación sin abrir las respuestas) → **dominado** (hiciste el laboratorio o lo aplicaste de verdad).

## Contenido

### Qué es
**Azure Machine Learning** (Azure ML) es el servicio de Azure para el ciclo completo de modelos de machine learning: preparar datos, entrenar (en tu PC o en cómputo en la nube con CPU o GPU), registrar experimentos y modelos, automatizar pipelines y desplegar modelos como endpoints. Es la plataforma de [[MLOps - conceptos]] de Azure para ML "clásico" y deep learning; para aplicaciones con LLMs y agentes, Microsoft orienta hacia Microsoft Foundry ([[Azure OpenAI y Azure AI Foundry]]), aunque ambos se integran.

### Cómo funciona
Piezas principales:
- **Workspace**: el recurso central. Agrupa experimentos, modelos, datos, cómputo y endpoints. Al crearlo se crean recursos asociados (Storage Account, Key Vault, Application Insights, Container Registry).
- **Cómputo**:
  - **Compute instance**: una VM de desarrollo personal con Jupyter y VS Code.
  - **Compute cluster**: nodos que escalan para trabajos de entrenamiento; puede escalar a 0 nodos cuando no hay trabajos.
  - **Serverless compute**: envías el job y Azure gestiona el cómputo.
- **Jobs**: ejecuciones de un script con su entorno y entradas; se definen en YAML o con el SDK.
- **Environments**: imagen de contenedor + dependencias, para reproducibilidad.
- **Data assets**: datos registrados con versión ([[Versionado de datos y modelos]]).
- **Registro de modelos** y compatibilidad con [[MLflow]]: el workspace expone un servidor de tracking MLflow, así que tu código con `mlflow.log_metric` registra ahí.
- **Pipelines**: varios pasos (preparar, entrenar, evaluar) encadenados.
- **Endpoints**: en línea (*managed online endpoints*, tiempo real) y por lotes (*batch endpoints*).
- **AutoML** y **Designer** para quien prefiere menos código.

Herramientas: **CLI v2** (`az ml`), **SDK de Python v2** (`azure-ai-ml`) y el portal **Azure ML Studio**. El SDK v1 (`azureml-core`) y la CLI v1 están retirados: Microsoft avisó que había que quitar la dependencia antes del 30 de junio de 2026. Ignora tutoriales que usen `azureml.core`.

```mermaid
flowchart LR
    Dev[Tu PC: código + YAML] -- az ml job create --> WS[Workspace]
    WS --> CC[Compute cluster 0..N nodos]
    CC --> MLF[Métricas y modelo en MLflow]
    MLF --> REG[Registro de modelos]
    REG --> EP[Managed online endpoint]
    EP --> App[Itzamna.Dionisio]
```

### Ejemplo
Enviar un entrenamiento a un clúster que escala a cero, con CLI v2:

```yaml
# job.yml
$schema: https://azuremlschemas.azureedge.net/latest/commandJob.schema.json
code: ./src
command: python train.py --datos ${{inputs.datos}}
inputs:
  datos:
    type: uri_file
    path: azureml:clientes-entrenamiento:3     # data asset versionado
environment: azureml:sklearn-env:1
compute: azureml:cpu-cluster
experiment_name: pago-tarde
```

```bash
az extension add -n ml
# Clúster que se apaga solo: mínimo 0 nodos y 5 minutos de inactividad
az ml compute create -n cpu-cluster --type AmlCompute --size Standard_DS3_v2 \
  --min-instances 0 --max-instances 2 --idle-time-before-scale-down 300 \
  -g rg-ml -w ws-ml
az ml job create -f job.yml -g rg-ml -w ws-ml
```

Un pipeline de Azure DevOps puede ejecutar el mismo `az ml job create` con una *service connection*, de modo que el entrenamiento queda versionado y automatizado.

#### Cuidar el cómputo (y la factura)
El servicio de Azure ML en sí no es lo caro; **pagas el cómputo y el almacenamiento** que usas (consulta la página de precios para tu región). Cuida:
- **Compute instances**: se cobran mientras estén encendidas, aunque no las uses. Activa el **apagado por inactividad** y programa horarios de apagado.
- **Clusters**: `min-instances 0` para que no queden nodos prendidos.
- **GPU**: son mucho más caras; úsalas solo cuando el entrenamiento lo justifique y pide cuota con anticipación.
- **Endpoints en línea**: cobran por instancia desplegada todo el tiempo; borra los de prueba.
- **Presupuestos y alertas** en Azure Cost Management.

#### Practicar sin costo (o casi)
- Usa el **crédito de la cuenta gratuita de Azure** para nuevas cuentas (monto y vigencia actuales: Pendiente de confirmar) o Azure for Students si aplicas.
- Entrena en local o en Google Colab y usa Azure ML solo para registrar con MLflow (el registro en sí cuesta muy poco).
- Usa la VM más pequeña, clusters con mínimo 0 y borra el grupo de recursos al terminar cada práctica.
- Los módulos de Microsoft Learn sobre Azure ML explican el flujo; algunos incluyen *sandbox*, otros requieren tu suscripción (revisa cada módulo).

### Cuándo usarlo y cuándo no
- **Úsalo** si tu organización está en Azure y necesitas entrenamiento en la nube, gobernanza, registro central y endpoints administrados.
- **No hace falta** para modelos pequeños que entrenas en minutos en tu PC y despliegas dentro de tu API con [[ML.NET]] u [[ONNX Runtime]].
- Para LLMs por API, agentes y RAG, empieza por Microsoft Foundry.

### Errores comunes
- **Dejar compute instances encendidas** el fin de semana.
- **Seguir tutoriales del SDK v1** (`azureml.core`), ya retirado.
- **Entornos sin versiones fijas**: un job deja de funcionar cuando cambia una librería.
- **Secretos en el código del job**: usa el Key Vault del workspace o identidades administradas.
- **No borrar recursos de prueba** (endpoints, clusters, workspaces).

### Para profundizar
- [Documentación de Azure Machine Learning (Microsoft Learn)](https://learn.microsoft.com/azure/machine-learning/)
- [Qué es la CLI y el SDK v2 (Microsoft Learn)](https://learn.microsoft.com/azure/machine-learning/concept-v2)
- [Administrar y optimizar costos en Azure ML (Microsoft Learn)](https://learn.microsoft.com/azure/machine-learning/how-to-manage-optimize-cost)

## Autoevaluación

Contesta primero en voz alta o en papel; luego despliega la respuesta.

> [!question]- ¿Qué ofrece Azure Machine Learning?
> Espacios de trabajo para datos, cómputo de entrenamiento, seguimiento de experimentos (compatible con MLflow), registro de modelos, pipelines y endpoints de despliegue.

> [!question]- ¿Qué cuidas con el cómputo?
> Apagar o escalar a cero las instancias y clústeres que no usas: el cómputo encendido se cobra.

> [!question]- ¿Cómo practicas sin costo?
> Con los sandbox y ejercicios de Microsoft Learn o una cuenta gratuita con crédito (condiciones vigentes: Pendiente de confirmar).

