# Sweeps Quickstart

Comienza a partir de cualquier modelo de aprendizaje de máquinas y obtén un barrido de hiperparámetros que se ejecuta en minutos. ¿Quieres ver un ejemplo en funcionamiento? Aquí está el [código de ejemplo](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion) y un [panel de control de ejemplo](https://app.wandb.ai/carey/pytorch-cnn-fashion/sweeps/v8dil26q).

![](../.gitbook/assets/image%20%2847%29%20%282%29%20%282%29.png)

{% hint style="info" %}
¿Ya tienes un proyecto en Weights & Biases? [Salta a nuestro próximo tutorial acerca de Barridos →](https://docs.wandb.ai/sweeps/existing-project)
{% endhint %}

## 1. Agrega wandb

### Establece tu cuenta

1. Comienza con una cuenta de W&B. [Crea una ahora →](http://app.wandb.ai/)
2. Ve al directorio de tu proyecto en tu terminal e instala nuestra biblioteca: `pip install wandb`
3. Dentro del directorio de tu proyecto, inicia sesión en W&B: `wandb login`

### Establece tu script de entrenamiento de Python

1. Importa nuestra biblioteca `wandb`  
2. Asegúrate de que tus hiperparámetros puedan ser establecidos apropiadamente por el barrido. Defínelos en un diccionario, al principio de tu script, y pásalos a wandb.init.
3. Registra métricas para verlas en el tablero de control en tiempo real

```python
import wandb

# Set up your default hyperparameters before wandb.init
# so they get properly set in the sweep
hyperparameter_defaults = dict(
    dropout = 0.5,
    channels_one = 16,
    channels_two = 32,
    batch_size = 100,
    learning_rate = 0.001,
    epochs = 2,
    )

# Pass your defaults to wandb.init
wandb.init(config=hyperparameter_defaults)
config = wandb.config

# Your model here ...

# Log metrics inside your training loop
metrics = {'accuracy': accuracy, 'loss': loss}
wandb.log(metrics)
```

 [Ver un ejemplo de código completo →](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)

## 2. Configuración del Barrido

Establece un archivo YAML para especificar a tu script de entrenamiento, los rangos de los parámetros, la estrategia de búsqueda y los criterios de detención. W&B va a pasar estos parámetros y sus valores como argumentos de la línea de comandos a tu script de entrenamiento, y automáticamente los vamos a analizar con el objeto de configuración que estableciste en el [Paso 1](https://docs.wandb.ai/sweeps/quickstart#set-up-your-python-training-script).

 Aquí hay algunos recursos de configuración:

1.  ****[Ejemplo de archivos YAML](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion): un script de ejemplo y varios archivos YAML diferentes
2. [Configuración](https://docs.wandb.ai/sweeps/configuration): especificaciones completas para establecer la configuración de tu barrido
3.  [Notebook de Jupyter](https://docs.wandb.ai/sweeps/python-api): establece la configuración de tu barrido con un diccionario de Python, en lugar de un archivo YAML
4.  [Genera la configuración desde la interfaz de usuario](https://docs.wandb.ai/sweeps/existing-project): toma un proyecto W&B existente y genera un archivo de configuración
5.  [Alimento en ejecuciones previas](https://docs.wandb.com/sweeps/existing-project#seed-a-new-sweep-with-existing-runs): toma las ejecuciones previas y agrégalas a un nuevo barrido

Aquí hay un archivo YAML de ejemplo, con la configuración del barrido, que se llama **sweep.yaml:** 

```text
program: train.py
method: bayes
metric:
  name: val_loss
  goal: minimize
parameters:
  learning_rate:
    min: 0.001
    max: 0.1
  optimizer:
    values: ["adam", "sgd"]
```

{% hint style="warning" %}
Si especificas una métrica para optimizar, asegúrate de que la estés registrando. En este ejemplo, tengo a **val\_loss** en mi archivo de configuración, así que tengo que registrar exactamente el nombre de esa métrica en mi script:

`wandb.log({"val_loss": validation_loss})`
{% endhint %}

Internamente, esta configuración de ejemplo va a utilizar el método de optimización de Bayes para elegir el conjunto de los valores de los hiperparámetros con los que se va a llamar a tu programa. Va a lanzar experimentos con la siguiente sintaxis:

```text
python train.py --learning_rate=0.005 --optimizer=adam
python train.py --learning_rate=0.03 --optimizer=sgd
```

{% hint style="info" %}
Si estás usando argparse en tu script, te recomendamos que uses guiones bajos en los nombres de tus variables, en lugar de guiones.
{% endhint %}

## 3. Inicializa un barrido

Nuestras coordenadas al servidor central entre todos los agentes que ejecutan el barrido. Establece un archivo de configuración del barrido y ejecuta este comando para empezar:

```text
wandb sweep sweep.yaml
```

 Este comando va a imprimir un **ID del barrido**, que incluye el nombre de la entidad y el nombre del proyecto. ¡Cópialo para usarlo en el próximo paso!

## 4. Lanza el\(los\) agente\(s\)

En cada máquina sobre la que te gustaría ejecutar el barrido, comienza un agente con el ID del barrido. Querrás usar el mismo ID del barrido para todos los agentes que estén ejecutando el mismo barrido.

En una shell en tu máquina, ejecuta el comando wandb agent, que le va a pedir al servidor que ejecute ciertos comandos:

```text
wandb agent your-sweep-id
```

Puedes correr wandb agent en múltiples máquinas, o en múltiples procesos sobre la misma máquina, y cada agente le pedirá al servidor central de Barridos de W&B por el próximo conjunto de hiperparámetros a ejecutar.

## 5. Visualiza los resultados

Abre tu proyecto para ver los resultados en tiempo real en el panel de control de los barridos.

 [Panel de control de ejemplo →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

![](../.gitbook/assets/image%20%2888%29.png)

