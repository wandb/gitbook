---
description: >-
  Llama a wandb.init() al principio de tu script para comenzar una nueva
  ejecución
---

# wandb.init\(\)

Llama a `wandb.init()` una vez al comienzo de tu script para inicializar un nuevo trabajo. Esto crea una nueva ejecución en W&B y lanza un proceso en segundo plano para sincronizar los datos.

* **Instalación Local**: Si necesitas una nube privada o una instancia local de W&B, mira nuestros ofrecimientos [Self Hosted](https://docs.wandb.ai/self-hosted).
*  **Entornos Automatizados:** La mayoría de estos ajustes también pueden ser controlados a través de las [Variables de Entorno](https://docs.wandb.ai/library/environment-variables). A menudo, esto es útil cuando estés corriendo trabajos en un cluster.

###  Documentación de Referencia

Verifica la documentación de referencia para los argumentos

{% page-ref page="../ref/init.md" %}

##  Preguntas Comunes

### Cómo lanzo múltiples ejecuciones desde un script?

Si estás intentando empezar múltiples ejecuciones desde un script, agrega dos cosas a tu código:

1. **run = wandb.init\(reinit=True\):** Usa este ajuste para permitir la reinicialización de la ejecuciones.
2. **run.finish\(\):** Usa esto al final de tu ejecución para finalizar el registro para dicha ejecución.

```python
import wandb
for x in range(10):
    run = wandb.init(project="runs-from-for-loop", reinit=True)
    for y in range (100):
        wandb.log({"metric": x+y})
    run.finish()
```

Alternativamente, puedes utilizar un administrador de contexto de python, que automáticamente finalizará el registro:

```python
import wandb
for x in range(10):
    run = wandb.init(reinit=True)
    with run:
        for y in range(100):
            run.log({"metric": x+y})
```

### LaunchError: Permission denied

Si estás obteniendo un error **LaunError: Launch exception:** Permission denied, es porque no tienes permiso para registrar al proyecto al que estás intentando enviar ejecuciones. Esto podría ocurrir por algunas razones diferentes.

1. No has iniciado sesión en esta máquina. Corre `wandb login` desde la línea de comandos.
2.  Has establecido una entidad que no existe. La “entidad” debería ser tu nombre de usuario o el nombre de un equipo existente. Si necesitas crear un equipo, ve a nuestra [página ](https://app.wandb.ai/billing)[de ](https://app.wandb.ai/billing)[Subscripciones](https://app.wandb.ai/billing).
3. No tienes permisos en el proyecto. Pídele al creador del proyecto que establezca la privacidad a Abierta, para que así puedas registrar las ejecuciones para este proyecto.

### Obtén el nombre de ejecución legible

Obtén el nombre de ejecución legible y amigable para tu ejecución.

```python
import wandb

wandb.init()
run_name = wandb.run.name
```

###  Establece el nombre de la ejecución al ID de la ejecución generado

Si quisieras sobrescribir el nombre de la ejecución \(como por ejemplo, snowy-owl-10\) con el ID de la ejecución \(como por ejemplo, qvlp96vk\), puedes usar este fragmento de código:

```python
import wandb
wandb.init()
wandb.run.name = wandb.run.id
wandb.run.save()
```

###  Guarda el git commit

Cuando wandb.init\(\) es llamado en tu script, automáticamente buscamos información del git para guardar un enlace a tu repositorio del SHA del último commit. La información del git debería mostrarse en tu [página de ejecución](https://docs.wandb.ai/app/pages/run-page#overview-tab). Si ves que no está apareciendo allí, asegúrate de que tu script, desde donde llamas de wandb.init\(\), esté ubicado en un directorio que tenga información de git.

El git commit y el comando utilizado para correr el experimento están visibles para que los veas, pero permanecen ocultos a los usuarios externos, así que si tienes un proyecto público, estos detalles seguirán siendo privados.

### Guarda el registro cuando estés desconectado

Por defecto, wandb.init\(\) comienza un proceso que sincroniza las métricas en tiempo real con nuestra aplicación almacenada en la nube. Si tu máquina está fuera de línea, o no tienes acceso a internet, aquí está cómo hay que correr wandb utilizando el modo offline, y sincronizar después.

Establece dos variables de entorno:

1. **WANDB\_API\_KEY**: Establece esto con un valor igual a la clave de la API de tu cuenta, en la [página de ajustes](https://app.wandb.ai/settings).
2. **WANDB\_MODE**: dryrun

 Aquí hay un ejemplo de cómo debería verse en tu script:

```python
import wandb
import os

os.environ["WANDB_API_KEY"] = YOUR_KEY_HERE
os.environ["WANDB_MODE"] = "dryrun"

config = {
  "dataset": "CIFAR10",
  "machine": "offline cluster",
  "model": "CNN",
  "learning_rate": 0.01,
  "batch_size": 128,
}

wandb.init(project="offline-demo")

for i in range(100):
  wandb.log({"accuracy": i})
```

Aquí hay una salida de ejemplo en la terminal:

![](../.gitbook/assets/image%20%2881%29.png)

Y una vez que tengo internet, corro un comando de sincronización para enviar ese directorio a la nube.

`wandb sync wandb/dryrun-folder-name`

![](../.gitbook/assets/image%20%2836%29.png)

