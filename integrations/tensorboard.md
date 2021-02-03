# TensorBoard

W&B soporta los parches de TensorBoard para registrar automáticamente todas las métricas de tu script a nuestros gráficos nativos.

```python
import wandb
wandb.init(sync_tensorboard=True)
```

Soportamos TensorBoard con todas las versiones de TensorFlow. Si estás usando TensorBoard con otro framework, W&B soporta TensorBoard &gt; 1.14 con PyTorch, así también como TensorBoardX.

### Métricas personalizadas

Si necesitas registrar métricas personalizadas adicionales, que no están siendo registradas a TensorBoard, puedes llamar a wandb.log en tu código con el mismo argumento step que está usando TensorFlow: es decir, `wandb.log({"custom": 0.8}, step=global_step)`

###  Configuración Avanzada

Si deseas más control sobre cómo es parcheado TensorBoard, puedes llamar a `wandb.tensorboard.patch`, en vez de pasar `sync_tensorboard=True` a init. Puedes pasar `tensorboardX=False` a este método para asegurarte de que el TensorBoard estándar sea parcheado, si estás usando TensorBoard &gt; 1.14 con PyTorch, puedes pasar pytorch=True para asegurarte de que este sea parcheado. Ambas opciones tienen valores predeterminados inteligentes, dependiendo de qué versiones de estas bibliotecas hayan sido importadas.

Por defecto, también sincronizamos los archivos tfevents y cualquier archivo .pbtxt. Esto nos permite lanzar una instancia de TensorBoard por ti. Verás una [pestaña de TensorBoard](https://www.wandb.com/articles/hosted-tensorboard) en la página de ejecuciones. Este comportamiento puede ser deshabilitado al pasar save=False a `wandb.tensorboard.patch`

```python
import wandb
wandb.init()
wandb.tensorboard.patch(save=False, tensorboardX=True)
```

### Sincronizar Ejecuciones Previas de TensorBoard

Si tienes archivos `tfevents` existentes, almacenados localmente, que ya fueron generados usando la integración de la biblioteca de wandb, y te gustaría importarlos en wandb, puedes ejecutar `wandb sync log_dir,` donde log\_dir es un directorio conteniendo los archivos `tfevents`.

También puedes ejecutar `wandb sync directory_with_tf_event_file`

```bash
"""This script will import a directory of tfevents files into a single W&B run.
You must install wandb from a special branch until this feature is merged
into the mainline:""" 
pip install --upgrade git+git://github.com/wandb/client.git@feature/import#egg=wandb
```

Puedes llamar a este script con `python no_image_import.py dir_with_tf_event_file`. Esto creará una ejecución simple en wandb, con métricas de los archivos de eventos en ese directorio. Si deseas ejecutarlo en muchos directorios, solo deberías correr este script una vez por ejecución, así que un cargador podría verse de la siguiente forma:

```python
import glob
for run_dir in glob.glob("logdir-*"):
  subprocess.Popen(["python", "no_image_import.py", run_dir],
                   stderr=subprocess.PIPE, stdout=subprocess.PIPE)
```

```python
import glob
import os
import wandb
import sys
import time
import tensorflow as tf
from wandb.tensorboard.watcher import Consumer, Event
from six.moves import queue

if len(sys.argv) == 1:
    raise ValueError("Must pass a directory as the first argument")

paths = glob.glob(sys.argv[1]+"/*/.tfevents.*", recursive=True)
root = os.path.dirname(os.path.commonprefix(paths)).strip("/")
namespaces = {path: path.replace(root, "")\
              .replace(path.split("/")[-1], "").strip("/")
              for path in paths}
finished = {namespace: False for path, namespace in namespaces.items()}
readers = [(namespaces[path], tf.train.summary_iterator(path)) for path in paths] 
if len(readers) == 0: 
    raise ValueError("Couldn't find any event files in this directory")

directory = os.path.abspath(sys.argv[1])
print("Loading directory %s" % directory)
wandb.init(project="test-detection")

Q = queue.PriorityQueue()
print("Parsing %i event files" % len(readers))
con = Consumer(Q, delay=5)
con.start()
total_events = 0

while True:

    # Consume 500 events at a time from all readers and push them to the queue
    for namespace, reader in readers:
        if not finished[namespace]:
            for i in range(500):
                try:
                    event = next(reader)
                    kind = event.value.WhichOneof("value")
                    if kind != "image":
                        Q.put(Event(event, namespace=namespace))
                        total_events += 1
                except StopIteration:
                    finished[namespace] = True
                    print("Finished parsing %s event file" % namespace)
                    break
    if all(finished.values()):
        break

print("Persisting %i events..." % total_events)
con.shutdown(); print("Import complete")
```

###  Google Colab y TensorBoard

 Para ejecutar comandos desde la línea de comandos en Colab, tienes que correr `!wandb sync directoryname`. Actualmente, la sincronización de TensorBoard no funciona en un entorno de una notebook para Tensorflow 2.1+. Puedes hacer que tu Colab utilice una versión anterior de TensorBoard, o puedes correr un script desde la línea de comandos con `!python your_script.py`.

## ¿Qué tiene de diferente W&B respecto a TensorBoard?

 Nos inspiramos en mejorar las herramientas de seguimiento de experimentos para todo el mundo. Cuando los cofundadores comenzaron a trabajar en W&B, se inspiraron en construir una herramienta para los usuarios de OpenAI insatisfechos con TensorBoard. Aquí hay algunas cosas en las que nos enfocamos para mejorar:

1. **Reproducir modelos**: Weights & Biases es bueno para la experimentación, la exploración, y la reproducción de los modelos en el futuro. No solo que capturamos las métricas, sino también los hiperparámetros y la versión del código, y podemos guardar los puntos de control de tu modelo para que tu proyecto sea reproducible.
2. **Organización automática:** Si le pasas un proyecto a un colaborador o te tomas unas vacaciones, W&B te facilita ver todos los modelos que hayas probado, así no pierdes horas volviendo a correr los viejos experimentos.
3. **Integración rápida y flexible:** Agrega W&B a tu proyecto en 5 minutos. Instala nuestro paquete Python, libre y de código abierto, y agrega un par de líneas a tu código, y cada vez que corras tu modelo vas a registrar las métricas y los registros.
4. **Tablero de control persistente y centralizado:** Dondequiera que entrenes tus modelos, ya sea en tu máquina local, tu cluster del laboratorio, o en instancias de spot en la nube, te ofrecemos el mismo tablero de control centralizado. No necesitas perder el tiempo copiando y organizando archivos de TensorFlow desde diferentes máquinas.
5. **Tabla poderosa:** Busca, filtra, ordena y agrupa resultados de diferentes modelos. Es fácil revisar miles de versiones de los modelos y encontrar al más adecuado para diferentes tareas. TensorBoard no funciona adecuadamente en proyectos grandes.
6. **Herramientas para la colaboración:** Utiliza W&B para organizar proyectos de aprendizaje de máquinas complejos. Es fácil compartir un enlace a W&B, y puedes utilizar equipos privados y hacer que todos envíen resultados a un proyecto compartido. También soportamos la colaboración a través de los repositorios – agrega visualizaciones interactivas y describe tu trabajo en markdown. Esta es una gran forma de mantener un trabajo registrado, de compartir resultados con tu supervisor, o de presentar resultados a tu laboratorio.

 Empieza con una [cuenta personal gratuita →](http://app.wandb.ai/)

