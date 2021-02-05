---
description: 'Datos versionados, modelos y resultados a través de tus procesos de trabajo'
---

# Artifacts

Utiliza los Artefactos de W&B para almacenar y hacer el seguimiento del conjunto de datos, de los modelos y de los resultados de la evaluación a través de los procesos de trabajo del aprendizaje de máquinas. Piensa en un artefacto como en un directorio de datos versionado. Puedes almacenar conjuntos de datos enteros directamente en los artefactos, o utilizar referencias a los artefactos que apunten a los datos en otros sistemas.

 [Explora un ejemplo de Artefactos](https://wandb.ai/wandb/arttest/reports/Artifacts-Quickstart--VmlldzozNTAzMDM) acerca del versionado de conjuntos de datos y de la administración del modelo con una notebook simple e interactiva, alojada en Google Colab.

![](../.gitbook/assets/keras-example.png)

##  Inicio rápido

[![](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/artifacts-quickstart)

### 1. Registra un artefacto

Inicializa una ejecución y registra un artefacto, por ejemplo una versión de un conjunto de datos que estés usando para entrenar un modelo.

```python
run = wandb.init(job_type="dataset-creation")
artifact = wandb.Artifact('my-dataset', type='dataset')
artifact.add_file('my-dataset.txt')
run.log_artifact(artifact)
```

### 2. Utiliza el artefacto

Comienza una nueva ejecución y toma ese artefacto guardado, por ejemplo para usar el conjunto de datos para entrenar un modelo.

```python
run = wandb.init(job_type="model-training")
artifact = run.use_artifact('my-dataset:latest')
artifact_dir = artifact.download()
```

### 3. Registra una nueva versión

 Si un artefacto cambia, vuelve a correr el mismo script de creación del artefacto. En este caso, imagina que cambian los datos en el archivo my-dataset.txt. El mismo script va a capturar perfectamente la nueva versión – vamos a comprobar la suma de verificación del artefacto, identificar qué es lo que ha cambiado, y hacer el seguimiento de la nueva versión. Si no cambia nada, no vamos a volver a subir ningún dato ni a crear una nueva versión.

```python
run = wandb.init(job_type="dataset-creation")
artifact = wandb.Artifact('my-dataset', type='dataset')
# Imagine more lines of text were added to this text file:
artifact.add_file('my-dataset.txt')
# Log that artifact, and we identify the changed file
run.log_artifact(artifact)
# Now you have a new version of the artifact, tracked in W&B
```

## Cómo funciona

Al usar nuestra API Artifacts, puedes registrar artefactos como salidas de las ejecuciones de W&B, o utilizar artefactos como entradas a las ejecuciones.

![](../.gitbook/assets/simple-artifact-diagram-2.png)

Puesto que una ejecución puede usar el artefacto de salida de otra ejecución como su entrada, los artefactos y las ejecuciones forman un grafo dirigido. No necesitas definir pipelines por adelantado. Solamente utiliza y registra artefactos, y nosotros vamos a unir todo.

Aquí hay un [artefacto de ejemplo](https://app.wandb.ai/shawn/detectron2-11/artifacts/model/run-1cxg5qfx-model/4a0e3a7c5bff65ff4f91/graph) donde puedes ver la vista del resumen del DAG, así también como una vista alejada de cada ejecución, de cada paso, y de cada versión del artefacto.

![](../.gitbook/assets/2020-09-03-15.59.43.gif)

 Para aprender cómo usar los Artefactos, revisa la [D](https://docs.wandb.com/artifacts/api)[ocumentación de la API Artifacts→](https://docs.wandb.com/artifacts/api)

## Video Tutorial para los Artefactos de W&B

Continúa con nuestro [tutorial](https://www.youtube.com/watch?v=Hd94gatGMic) interactivo y aprende cómo hacer el seguimiento de tu proceso de trabajo para el aprendizaje de máquinas con los Artefactos de W&B

![](../.gitbook/assets/wandb-artifacts-video.png)

