---
description: wandb.keras
---

# Keras Reference

[fuente](https://github.com/wandb/client/blob/master/wandb/keras/__init__.py#L148)

```python
WandbCallback(self,
              monitor='val_loss',
              verbose=0,
              mode='auto',
              save_weights_only=False,
              log_weights=False,
              log_gradients=False,
              save_model=True,
              training_data=None,
              validation_data=None,
              labels=[],
              data_type=None,
              predictions=36,
              generator=None,
              input_type=None,
              output_type=None,
              log_evaluation=False,
              validation_steps=None,
              class_colors=None,
              log_batch_frequency=None,
              log_best_prefix='best_')
```

WandbCallback integra automáticamente a keras con wandb.

 **Ejemplos:**

```python
model.fit(X_train, y_train,  validation_data=(X_test, y_test),
callbacks=[WandbCallback()])
```

WandbCallback registrará automáticamente datos del historial de cualquier métrica recogida por keras: pérdida y cualquier cosa pasada a keras\_model.compile\(\).

WandbCallback va a establecer métricas de la síntesis para la ejecución asociada con el paso del “mejor” entrenamiento, en donde “mejor” está definido por los atributos `monitor` y `mode`. Esto deja a la época con un valor por defecto igual al mínimo de val\_loss. WandbCallback va a guardar por defecto al modelo asociado con la mejor época.

WandbCallback opcionalmente puede registrar histogramas de gradientes y parámetros.

WandbCallback opcionalmente puede guardar los datos del entrenamiento y de la validación para que wandb los visualice.

**Argumentos:**

* `monitor str` – nombre de la métrica que hay que monitorear. El valor predeterminado es val\_loss.
* `mode str` – uno de {“auto”, “min”, “max”}. “min” - guarda el modelo cuando el monitor es minimizado; “max” - guarda el modelo cuando el monitor es maximizado; “auto” - intenta darse cuenta de cuándo guardar el modelo \(por defecto\).
* `save_model`: True – guarda un modelo cuando el monitor supera todas las épocas previas. False – no guarda los modelos.
*  `save_weights_only boolean` – si es True, entonces van a ser guardados solamente los pesos del modelo \(model.save\_weights\(filepath\)\), sino el modelo completo \(model.save\(filepath\)\).
* `low_weights` – \(booleano\) si es True guarda los histogramas de los pesos de las capas del modelo.
* `log_gradients` – \(booleano\) si es True registra los histogramas de los gradientes del entrenamiento. El modelo debe definir un total\_loss.
*  `training_data` – \(tupla\) El mismo formato \(X,y\) que se le pasa a mode.fit. Esto es necesario para calcular los gradientes. Es obligatorio si log\_gradients es True.
* `validation_data` – \(tupla\) El mismo formato \(X,y\) que se le pasa a mode.fit. Un conjunto de datos para que visualice wandb. Si está establecido, en cada época, wandb hará un pequeño número de predicciones y guardará los resultados para una visualización posterior.
*  `generator generator` – un generador que devuelve datos de validación para que wandb visualice. Este generador debería devolver tuplas \(X,y\). Debería ser establecido ya sea validate\_data o generator para que wandb visualice ejemplos de datos específicos.
* `validation_steps` int – si validation\_data es un generador, cuántos pasos hay que correr al generador para que la validación completa sea establecida.
* `labels lista` – Si estás visualizando tus datos con wandb, esta lista de etiquetas va a convertir la salida numérica a strings entendibles, si es que estás construyendo un clasificador de múltiples clases. Si estás haciendo un clasificador binario, puedes pasarle una lista de dos etiquetas \[“etiqueta para falso”, “etiqueta para verdadero”\]. Si tanto validate\_data como generator son falsos, no va a hacer nada.
*  `predictions int` – el número de predicciones que hay que realizar para la visualización de cada época. El máximo es 100.
*  `input_type` string – tipo de la entrada del modelo para ayudar a la visualización. Puede ser uno de estos: \(“image”, “images”, “segmentation\_mask”\).
* `output_type` string - tipo de la salida del modelo para ayudar la visualización. Puede ser uno de estos: \(“image”, “images”, “segmentation\_mask”\).
*  `log_evaluation` booleano – Si es True guarda un dataframe que contiene los resultados de validación completos al final del entrenamiento.
*  `class_colors` \[float, float, float\] – si la entrada o la salida es una máscara de segmentación, es un arreglo conteniendo una tupla rgb \(en el rango 0-1\) por cada clase.
* `log_batch_frequency` intreger – si en None, el callback va a registrar cada época. Si es establecido a un entero, el callback va a registrar las métricas de entrenamiento cada uno de los  log\_batch\_frequency lotes.
* `log_best_prefix` string – si es None, no va a ser guardada ninguna métrica extra de la síntesis. Si es establecido a string, la métrica y la época monitoreadas serán antepuestas con este valor y van a ser almacenadas como métricas de síntesis.

