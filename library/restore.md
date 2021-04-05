---
description: >-
  Restituye un archivo, tal como un punto de control del modelo, en tu
  directorio de ejecución local para poder accederlo desde tu script.
---

# wandb.restore\(\)

## Resumen

Llamar a `wandb.restore(filename)` restituirá un archivo en tu directorio de ejecuciones local. Típicamente, `filename` se refiere a un archivo generado por una ejecución de un experimento anterior y que fue subido a nuestra nube. Esta llamada hará una copia local del archivo y devolverá un stream del archivo local, abierto para su lectura.

`wandb.restore` acepta algunos argumentos de palabras claves opcionales:

* **run\_path** — string que se refiere a una ejecución anterior a partir de la que se tomó el archivo, formateado como_'$ENTITY\_NAME/$PROJECT\_NAME/$RUN\_ID'_  or _'$PROJECT\_NAME/$RUN\_ID'_ \(por defecto: entidad actual, nombre del proyecto, y id de la ejecución\)
* **replace** — booleano que especifica si hay que sobrescribir una copia local del nombre del archivo con la copia de la nube, si resulta que la copia local está disponible \(por defecto: False\)
* **root** — string que especifica el directorio en el que hay que almacenar la copia local del archivo. Por defecto, se trata del directorio en el que se está trabajando actualmente, o `wandb.run.dir` si wandb.init fue llamado anteriormente \(por defecto: “.”\)

Casos de uso comunes:

* restituir la arquitectura del modelo o los pesos generados por las ejecuciones pasadas
*  reanudar el entrenamiento a partir del último punto de control en el caso de una falla \(ver la sección sobre la [reanudación](https://docs.wandb.ai/library/resuming) para obtener detalles críticos\)

## Ejemplos

 Mira [este reporte](https://app.wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W%26B--Vmlldzo3MDQ3Mw) para ver un ejemplo completo en funcionamiento.

```python
# restore a model file from a specific run by user "vanpelt" in "my-project"
best_model = wandb.restore('model-best.h5', run_path="vanpelt/my-project/a1b2c3d")

# restore a weights file from a checkpoint
# (NOTE: resuming must be configured if run_path is not provided)
weights_file = wandb.restore('weights.h5')
# use the "name" attribute of the returned object
# if your framework expects a filename, e.g. as in Keras
my_predefined_model.load_weights(weights_file.name)
```

> Si no especificas run\_path, necesitarás configurar la [reanudación](https://docs.wandb.ai/library/resuming) para tus ejecuciones. Si deseas acceder programáticamente a los archivos fuera del entrenamiento, utiliza la [API Run](https://docs.wandb.ai/library/restore).

