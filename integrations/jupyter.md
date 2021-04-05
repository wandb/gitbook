# Jupyter

 Utiliza Weight & Biases en tus notebooks de Jupyter para obtener visualizaciones interactivas y para hacer análisis personalizados sobre las ejecuciones de entrenamiento.

##  ****Casos de Uso para W&B con notebooks de Jupyter

1. **Experimentación iterativa:** Corre y vuelve a correr los experimentos, afinando parámetros, y obtén todas las ejecuciones que hayas guardado automáticamente a W&B, sin tener que tomar notas de forma manual en el camino.

   2. **Guardado código:** Cuando se reproduce un modelo, es difícil saber que celdas de la notebooks se ejecutaron, y en qué orden. Activa el guardado de código en tu [página de ajustes](https://app.wandb.ai/settings) para guardar un registro de la ejecución de las celdas por cada experimento.

   3. **Análisis personalizado:** Una vez que las ejecuciones son registradas en W&B, es fácil obtener un dataframe desde la API y hacer análisis personalizados, y entonces registrar aquellos resultados en W&B para guardarlos y compartirlos en los reportes.

##  Configurando notebooks

Comienza tu notebook con el siguiente código para instalar W&B y enlaza tu cuenta:

```python
!pip install wandb -qqq
import wandb
wandb.login()
```

A continuación, establece tu experimento y guarda los hiperparámetros:

```python
wandb.init(project="jupyter-projo",
           config={
               "batch_size": 128,
               "learning_rate": 0.01,
               "dataset": "CIFAR-100",
           })
```

 Después de ejecutar `wandb.init()`, comienza una nueva celda con `%%wandb` para ver los gráficos en tiempo real en la notebook. Si ejecutas esta celda múltiples veces, los datos serán anexados a la ejecución.

```python
%%wandb

# Your training loop here
```

Prueba por ti mismo en el [script de este ejemplo simple →](https://bit.ly/wandb-jupyter-widgets-colab)

![](../.gitbook/assets/jupyter-widget.png)

Después de ejecutar `wandb.init()`, comienza una nueva celda con `%%wandb` para ver los gráficos en tiempo real en la notebook. Si ejecutas esta celda múltiples veces, los datos serán anexados a la ejecución.

```python
# Initialize wandb.run first
wandb.init()

# If cell outputs wandb.run, you'll see live graphs
wandb.run
```

##  Características adicionales de Jupyter en W&B

1.  Colab: Cuando llamas a wandb.init\(\) por primera vez en un Colab, automáticamente autenticamos tu runtime si actualmente has iniciado sesión con W&B desde el navegador. En la pestaña resumen de tu página de ejecuciones, verás un enlace a Colab. Si activas el guardado de código en los [ajustes](https://app.wandb.ai/settings), también puedes ver las celdas que fueron ejecutadas para correr el experimento, permitiendo una mejor reproducibilidad.
2. Lanzar Docker Jupyter: Llama a `wandb docker –jupyter` para lanzar un contender de docker, montar tu código en él, asegurarte de que Jupyter sea instalado y lanzarlo en el `puerto 8888`.
3. run.finish\(\): Por defecto, esperamos hasta la próxima vez que `wandb.init()` se llamado para marcar a una ejecución como finalizada. Esto te permite ejecutar celdas individuales y hacer que todas se registren en la misma ejecución. Para marcar manualmente a una ejecución como completada en una notebook de Jupyter, utiliza la característica run.finish\(\).

```python
import wandb
run = wandb.init()
# Training script and logging goes here
run.finish()
```

###  ****Silenciar los mensajes de información de W&B

Para deshabilitar los mensajes de información, ejecuta lo siguiente en una celda de la notebook:

```python
import logging
logger = logging.getLogger("wandb")
logger.setLevel(logging.ERROR)
```

## Preguntas Comunes

### Nombre de la notebook

Si estás viendo el mensaje de error "Failed to query for notebook name, you can set it manually with the WANDB\_NOTEBOOK\_NAME environment variable", puedes resolverlo al establecer la variable de entorno desde tu script de esta manera: `os.environ['WANDB_NOTEBOOK_NAME'] = 'some text here'`

