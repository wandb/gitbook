# Resuming

Puedes hacer que wandb reanude automáticamente las ejecuciones al pasarle `resume=True` a `wandb.init()`. Si tu proceso no termina exitosamente, la próxima vez que lo corras wandb comenzará el registro desde el último paso. Debajo hay un ejemplo simple en Keras:

```python
import keras
import numpy as np
import wandb
from wandb.keras import WandbCallback
wandb.init(project="preemptable", resume=True)

if wandb.run.resumed:
    # restore the best model
    model = keras.models.load_model(wandb.restore("model-best.h5").name)
else:
    a = keras.layers.Input(shape=(32,))
    b = keras.layers.Dense(10)(a)
    model = keras.models.Model(input=a,output=b)

model.compile("adam", loss="mse")
model.fit(np.random.rand(100, 32), np.random.rand(100, 10),
    # set the resumed epoch
    initial_epoch=wandb.run.step, epochs=300,
    # save the best model if it improved each epoch
    callbacks=[WandbCallback(save_model=True, monitor="loss")])
```

La reanudación automática solo funciona si el proceso es reanudado sobre el mismo sistema de archivos que el proceso fallido. Si no puedes compartir un sistema de archivos, te permitimos establecer **WANDB\_RUN\_ID**: un string globalmente único \(por proyecto\), correspondiente a una ejecución simple de tu script. No debe tener más de 64 caracteres. Todos los caracteres que no sean alfanuméricos serán convertidos a guiones.

```python
# store this id to use it later when resuming
id = wandb.util.generate_id()
wandb.init(id=id, resume="allow")
# or via environment variables
os.environ["WANDB_RESUME"] = "allow"
os.environ["WANDB_RUN_ID"] = wandb.util.generate_id()
wandb.init()
```

 Si estableces **WANDB\_RESUME** a “allow”, siempre puedes establecer **WANDB\_RUN\_ID** a un string único, y las reanudaciones del proceso serán manejadas de forma automática. Si estableces **WANDB\_RESUME** igual a “must”, wandb lanzará un error si la ejecución a reanudarse no existe aún, en lugar de crear automáticamente una nueva.

| Método | Sintaxis | No Reanudar Nunca \(defecto\) | Reanudar Siempre | Reanudar especificando el id de la ejecución | Reanudar desde el mismo directorio |
| :--- | :--- | :--- | :--- | :--- | :--- |
| línea de comandos | wandb run --resume= | "never" | "must" | "allow" \(Requires WANDB\_RUN\_ID=RUN\_ID\) | \(no disponible\) |
| entorno | WANDB\_RESUME= | "never" | "must" | "allow" \(Requires WANDB\_RUN\_ID=RUN\_ID\) | \(no disponible\) |
| init | wandb.init\(resume=\) |  | \(not available\) | resume=RUN\_ID | resume=True |

{% hint style="warning" %}
Si múltiples procesos usan el mismo run\_id concurrentemente, serán registrados resultados inesperados y se producirá una limitación de la frecuencia
{% endhint %}

{% hint style="info" %}
Si reanudas una ejecución y tienes notas especificadas en wandb.init\(\), aquellas notas sobrescribirán cualquier nota que hayas agregado en la Interfaz de Usuario.
{% endhint %}

