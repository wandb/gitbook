# Common Questions

## Estableciendo el proyecto y la entidad

Cuando ejecutas el comando para iniciar un barrido, tienes que darle a dicho comando un proyecto y una entidad. Si deseas uno diferente, esto puede ser especificado de 4 formas:

1. argumentos de la línea de comandos para `wandb sweep` \(`--project` and `--entity` arguments\)
2. archivo de ajustes de wandb \(`project` and `entity` keys\)
3. [configuración del barrido](https://docs.wandb.ai/sweeps/configuration) \(`project` and `entity` keys\) 
4. [variables de entorno](https://docs.wandb.ai/library/environment-variables) \(`WANDB_PROJECT` and `WANDB_ENTITY` variables\)

## Los agentes del barrido se detienen después de que las primeras ejecuciones finalizan

`wandb: ERROR Error while calling W&B API: anaconda 400 error: {"code":400,"message":"TypeError: bad operand type for unary -: 'NoneType'"}`

Una razón común para esto es que la métrica que estás optimizando en tu archivo de configuración YAML no es una métrica que estés registrando. Por ejemplo, podrías estar optimizando la métrica **f1**, pero registrando a validation\_**f1**. Vuelve a revisar para comprobar que estés registrando el nombre de la métrica exacta que intentas optimizar.

## Establece un número de ejecuciones para probar

La búsqueda aleatoria correrá para siempre hasta que tú detengas el barrido. Puedes establecer un objetivo para detener automáticamente el barrido cuando éste consiga un cierto valor para una métrica, o puedes especificar el número de ejecuciones que debería intentar un agente: `wandb agent --count NUM SWEEPID`

wandb agent --count NUM SWEEPID

### Ejecuta un barrido en Slurm

Te recomendamos correr `wandb agent --count 1 SWEEP_ID,` que va a ejecutar un trabajo de entrenamiento simple y entonces va a salir..

## Vuelve a ejecutar la búsqueda en rejilla

Si agotas la búsqueda en rejilla, pero quieres volver a correr algunas de las ejecuciones, puedes borrar aquellas que quieras volver a correr, entonces apretar el botón reanudar, en la página de control del barrido, y entonces iniciar nuevos agentes para ese ID de barrido.

## Los Barridos y las Ejecuciones deben estar en el mismo proyecto

`wandb: WARNING Ignoring project='speech-reconstruction-baseline' passed to wandb.init when running a sweep`

Puedes establecer un proyecto con wandb.init\(\) cuando estás ejecutando un barrido. El barrido y las ejecuciones tienen que estar en el mismo proyecto, así que el proyecto es establecido durante la creación del barrido: wandb.sweep\(sweep\_config, project=“fdsfsdfs”\)

## Error uploading

Si estás viendo **ERROR Error uploading &lt;file&gt;: CommError, Run does not exist**, podrías estar estableciendo un ID para tu ejecución, `wandb.init(id="some-string").` Este ID necesita ser único en el proyecto, y si no lo es, lanzará un error. En el contexto de los barridos, no puedes establecer un ID de forma manual para tus ejecuciones, puesto que nosotros somos quienes generamos automáticamente los IDs aleatorios y únicos para las ejecuciones.

Si estás intentando obtener un nombre bonito para mostrar en la tabla y en los gráficos, te recomendamos que uses name en lugar de id. Por ejemplo:

```python
wandb.init(name="a helpful readable run name")
```

### Barrido con comandos personalizados

Si normalmente corres el entrenamiento con un comando y los argumentos, por ejemplo:

```text
edflow -b <your-training-config> --batch_size 8 --lr 0.0001
```

Puedes convertirlo a una configuración de barridos, de esta forma:

```text
program:
  edflow
command:
  - ${env}
  - python
  - ${program}
  - "-b"
  - your-training-config
  - ${args}
```

La clave ${args} se expande a todos los parámetros en el archivo de configuración del barrido, expandidos de esta forma pueden ser analizados por argparse: --param1 value1 --param2 value2.

Si tienes argumentos extras, que no quieres especificar con argparse, 

puedes usar:parser = argparse.

ArgumentParser\(\)

args, unknown = parser.parse\_known\_args\(\)

**Ejecutando Barridos con Python 3**

Si estás teniendo algún problema, en donde el barrido está intentando usar Python 2, es fácil especificar que en su lugar éste debería usar Python 3. Sólo agrega esto a tu archivo YAML de la configuración del barrido:

```text
program:
  script.py
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```

\*\*\*\*

## Detalles de la optimización Bayesiana

El modelo del proceso Gaussiano, que es utilizado por la optimización Bayesiana, está definido en nuestra [lógica de barridos de código abierto](https://github.com/wandb/client/tree/master/wandb/sweeps). Si quisieras configuraciones y controles adicionales, prueba con el soporte para [Ray Tune](https://docs.wandb.com/sweeps/ray-tune).

Utilizamos un [kernel Matern](https://scikit-learn.org/stable/modules/generated/sklearn.gaussian_process.kernels.Matern.html), que es una generalización de RBF – definido [aquí](https://github.com/wandb/client/blob/541d760c5cb8776b1ad5fcf1362d7382811cbc61/wandb/sweeps/bayes_search.py#L30), en nuestro código abierto.

## Pausando barridos vs. deteniendo barridos con wandb.agent 

 ¿Hay alguna forma de decirle a `wandb.agent` que termine cuando no haya más trabajos disponibles? Porque he pausado el barrido.

Si detienes el barrido, en lugar de pausarlo, entonces los agentes terminarán. Al pausar, queremos que los agentes sigan corriendo, así el barrido puede ser restituido sin tener que lanzar al agente nuevamente.

## Forma recomendada de establecer los parámetros de configuración en un barrido

`wandb.init(config=config_dict_that_could_have_params_set_by_sweep)`  
o  
`experiment = wandb.init()    
experiment.config.setdefaults(config_dict_that_could_have_params_set_by_sweep)`

La ventaja de hacerlo así es que va a ignorar los ajustes que cualquier clave ya haya establecido para el barrido.

##  ¿Hay alguna forma de agregar un valor categórico adicional a un barrido, o necesito comenzar uno nuevo?

Una vez que un barrido ha empezado, no puedes cambiar la configuración del mismo, pero puedes ir a cualquier vista de la tabla y usar las casillas de selección para seleccionar las ejecuciones, entonces utilizar la opción del menú “crear barrido”, para crear un nuevo barrido usando las ejecuciones previas.

