---
description: Guarda un archivo en la nube que está asociado con la ejecución actual
---

# wandb.save\(\)

Hay dos formas de guardar un archivo asociado con una ejecución

1. Usar `wandb.save(filename)`.
2.  Poner un archivo en el directorio de ejecución de wandb, y subirlo al final de la ejecución.

{% hint style="info" %}
Si estás [reanudando](https://docs.wandb.ai/library/resuming) una ejecución, puedes recuperar un archivo al llamar a wandb.restore\(filename\)
{% endhint %}

Si deseas sincronizar los archivos a medida que estos son escritos, puedes especificar un nombre de archivo o un glob en `wandb.save`.

## Ejemplos de wandb.save

Mira [este reporte](https://app.wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W%26B--Vmlldzo3MDQ3Mw) para ver un ejemplo completo funcionando.

```python
# Save a model file from the current directory
wandb.save('model.h5')

# Save all files that currently exist containing the substring "ckpt"
wandb.save('../logs/*ckpt*')

# Save any files starting with "checkpoint" as they're written to
wandb.save(os.path.join(wandb.run.dir, "checkpoint*"))
```

{% hint style="info" %}
Los directorios de ejecución locales de W&B, por defecto están dentro del directorio ./wandb, relativo a tu script, y la ruta se ve algo así como run-20171023\_105053-3o4933r0, donde 20171023\_105053 es el timestamp y 3o4933r0 es el ID de la ejecución. Puedes establecer la variable de entorno WANDB\_DIR, o el argumento de la palabra clave dir de wandb.init a una ruta absoluta, y los archivos van a ser escritos en ese directorio de ahí en adelante.
{% endhint %}

## Ejemplo de guardar un archivo al directorio de ejecución wandb

El archivo “model.h5” es guardado en wandb.run.dir y será subido al final del entrenamiento.

```python
import wandb
wandb.init()

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
    callbacks=[wandb.keras.WandbCallback()])
model.save(os.path.join(wandb.run.dir, "model.h5"))
```

 Aquí hay una página de ejemplos pública. Puedes ver que en la pestaña Files está el model-best.h5. Por defecto, este es guardado automáticamente por la integración de Keras, pero puedes guardar un punto de control manualmente, y lo almacenaremos por ti junto con tu ejecución.

[ Ver el ejemplo en tiempo real →](https://wandb.ai/wandb/neurips-demo/runs/206aacqo/files)

![](../.gitbook/assets/image%20%2839%29%20%286%29%20%281%29%20%285%29.png)

##  Preguntas Comunes

###  Ignorando ciertos archivos

Puedes editar el archivo `wandb/settings` y establecer ignore\_globs igual a una lista de [globs](https://en.wikipedia.org/wiki/Glob_%28programming%29) separados por una coma. También puedes establecer la variable de entorno **WANDB\_IGNORE\_GLOBS**. Un caso de uso común es para evitar que sea subido el parche de git que creamos automáticamente. Es decir,  **WANDB\_IGNORE\_GLOBS=\*.patch**

### Sincronizar archivos antes de terminar la ejecución

 Si tienes una ejecución larga, puede que quieras ver que ciertos archivos, como los puntos de control del modelo, sean subidos a la nube antes de que finalice la ejecución. Por defecto, esperamos subir la mayoría de los archivos al final de la ejecución. Puedes agregar `wandb.save('*.pth')` o `wandb.save('latest.pth')` en tu script para subir estos archivos toda vez que los mismos sean escritos o actualizados.

### Cambiar el directorio para guardar archivos

Si por defecto guardas los archivos en AWS S3 o Google Cloud Storage, puede ser que veas este error:`events.out.tfevents.1581193870.gpt-tpu-finetune-8jzqk-2033426287 is a cloud storage url, can't save file to wandb.`

Para cambiar el directorio de los registros para los archivos de eventos de TensorBoard u otros archivos que te gustaría que sincronicemos, guarda tus archivos en wandb.run.dir, así son sincronizados con nuestra nube.

###  ****Obtén el nombre de la ejecución

Si te gustaría usar el nombre de la ejecución desde dentro de tu script, puedes usar `wandb.run.name` y así obtendrás el nombre de la ejecución – por ejemplo, "blissful-waterfall-2".

Necesitas llamar a save sobre el objeto ejecución antes de ser capaz de visualizar el nombre:

```text
run = wandb.init(...)
run.save()
print(run.name)
```

###  Pon todos los archivos guardados en wandb

Llama a `wandb.save(“*.pt”)` una vez al comenzar tu script, después de wandb.init, entonces todos los archivos que se correspondan con ese patrón se van a guardar inmediatamente una vez que sean escritos a wandb.run.dir.

### Elimina los archivos locales que hayan sido sincronizados al almacenamiento de la nube

Hay un comando, `wandb gc`, que puedes ejecutar para eliminar los archivos locales que ya hayan sido sincronizados con el almacenamiento de la nube. Se puede encontrar más información con el comando \`wandb gc -help

