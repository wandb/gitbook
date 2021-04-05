---
description: >-
  Weights & Biases para el seguimiento de experimentos, el versionado de los
  conjuntos de datos y la gestión de modelos
---

# Library

Utiliza la biblioteca de Python `wandb` para hacer el seguimiento de los experimentos de aprendizaje de máquinas con unas pocas líneas de código. Si estás usando un framework popular como  [PyTorch](../integrations/pytorch.md) o [Keras](../integrations/keras.md), nosotros tenemos[ integraciones ](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSSO0OxJem4ciGbHImo/v/espanol/integrations)livianas.

##  Integrando W&B en tu script

 Debajo hay bloques de construcción simples para hacer el seguimiento de un experimento con W&B. También tenemos un sinnúmero de integraciones especiales para [PyTorch](../integrations/pytorch.md), [Keras](../integrations/keras.md), [Scikit](../integrations/scikit.md), etc. Ver las [Integraciones](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSSO0OxJem4ciGbHImo/v/espanol/integrations).

1. \*\*\*\*[**wandb.init\(\)**](init.md):  Inicializa una nueva ejecución al principio de tu script. Esto devuelve un objeto Run y crea un directorio local donde son guardados todos los registros y los archivos, que luego son transmitidos asincrónicamente a un servidor W&B. Si deseas utilizar un servidor privado, en lugar de nuestro servidor almacenado en la nube, ofrecemos [Self-Hosting](../self-hosted/). 
2. \*\*\*\*[**wandb.config**](config.md): Guarda un diccionario de hiperparámetros, tales como la tasa de aprendizaje o el tipo de modelo. Los ajustes del modelo que capturas en la configuración después son útiles para organizar y consultar tus resultados.
3. \*\*\*\*[**wandb.log\(\)**](log.md): Registra métricas a lo largo del tiempo en un ciclo de entrenamiento, tales como la precisión y la pérdida. Por defecto, cuando llamas a wandb.log\(\), éste anexa un nuevo paso al objeto history y actualiza al objeto summary.
   * **history:** Un arreglo de objetos de tipo diccionario que hace un seguimiento de las métricas a lo largo del tiempo. Estos valores de las series de tiempos son mostrados como diagramas de líneas, por defecto, en la Interfaz de Usuario.
   * **summary:** Por defecto, el valor final de una métrica registrada con wandb.log\(\). Puedes establecer el summary para una métrica manualmente, para capturar la precisión más alta o la pérdida más baja, en lugar del valor final. Estos valores son usados en la tabla, y en los diagramas que comparan las ejecuciones – por ejemplo, podrías visualizar la precisión final para todas las ejecuciones de tu proyecto.
4. \*\*\*\*[**Artefactos:**](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSSO0OxJem4ciGbHImo/v/espanol/artifacts) ****Guarda las salidas de una ejecución, como los pesos del modelo o la tabla de predicciones. Esto te permite hacer un seguimiento no solo del entrenamiento del modelo, sino también de todos los pasos del proceso que afectan al modelo final.

## Mejores Prácticas

 La biblioteca `wandb`es increíblemente flexible. Aquí hay algunas guías sugeridas.

1. **Config**: Hace el seguimiento de los hiperparámetros, la arquitectura, los conjunto de datos, y todo lo demás que te gustaría usar para reproducir tu modelo. Todo esto se va a mostrar en columnas – utiliza columnas de configuración para agrupar, ordenar y filtrar ejecuciones de forma dinámica en la aplicación.
2. **Project**: Un proyecto es un conjunto de experimentos que puedes comparar en forma conjunta. Cada proyecto obtiene una página dedicada en el tablero de control, y puedes activar o desactivar con facilidad diferentes grupos de ejecuciones para comparar diferentes versiones del modelo.
3. **Notes**: Un mensaje de referencia rápido para ti mismo, la nota puede ser establecida desde tu script y es editable en la tabla.
4. **Tags**: Identifica las ejecuciones de referencia y las favoritas. Puedes filtrar ejecuciones al usar etiquetas, y estas son editables en la tabla.

```python
import wandb

config = dict (
  learning_rate = 0.01,
  momentum = 0.2,
  architecture = "CNN",
  dataset_id = "peds-0192",
  infra = "AWS",
)

wandb.init(
  project="detect-pedestrians",
  notes="tweak baseline",
  tags=["baseline", "paper1"],
  config=config,
)
```

##  ¿Qué datos son registrados?

All the data logged from your script is saved locally to your machine in a **wandb** directory, then sync'd to the W&B cloud or your [private server](../self-hosted/). Todos los datos registrados desde tu script son guardados localmente a tu máquina, en un directorio wandb, y entonces son sincronizados con la nube W&B o con tu servidor privado.

### **Logged Automatically**

* **Métricas del sistema:** utilización de CPU y de GPU, de red, etc. Estas vienen de [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface)  y son mostradas en la pestaña Sistema en la página de ejecución.
*  **Línea de comandos**: Lo que va a stdout y stderr es recogido y mostrado en la pestaña logs, en la página de ejecución.

Activa [Guardar Código](http://wandb.me/code-save-colab) en la página Ajustes de tu cuenta para obtener:

* **Git commit**: Selecciona el último git commit y míralo en la pestaña Resumen de la página de ejecución, así también como un archivo diff.patch, si hay algún cambio no confirmado.
*  **Archivos:** el archivo `requirements.txt` será subido y mostrado en la pestaña Archivos de la página de ejecución, conjuntamente con cualquier archivo que guardes en el directorio wandb para la ejecución.

### Registrado con llamadas específicas

En lo que respecta a los datos y a las métricas de los modelos, tienes que decidir qué es exactamente lo que quieres registrar.

* **Conjuntos de datos**: Tienes que registrar específicamente imágenes u otras muestra de los conjuntos de datos para que estos se transmitan a W&B.
* **Gradientes de PyTorch**: Agrega `wandb.watch(model)` para ver los gradientes de los pesos como histogramas en la Interfaz de Usuario.
* **Configuración**: Registra hiperparámetros, un enlace a tu conjunto de datos, o el nombre de la arquitectura que estás usando, como parámetros de configuración, pasados de esta forma: `wandb.init(config=your_config_dictionary)`.
*  **Métricas:** Utiliza `wandb.log()` para ver las métricas de tu modelo. Si registras métricas, como la precisión o la pérdida, desde dentro de tu ciclo de entrenamiento, obtendrás en la Interfaz de Usuario gráficos actualizados en tiempo real.

