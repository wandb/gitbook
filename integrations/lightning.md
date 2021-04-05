---
description: Visualize PyTorch Lightning models with W&B
---

# PyTorch Lightning

PyTorch Lightning provee un wrapper liviano para organizar tu código PyTorch y para agregar fácilmente características avanzadas tales como [entrenamiento distribuido](https://pytorch-lightning.readthedocs.io/en/latest/multi_gpu.html) y [precisión de 16 bits](https://pytorch-lightning.readthedocs.io/en/latest/amp.html)       . W&B provee un wrapper liviano para registrar tus experimentos de ML. Estamos incorporados directamente en la biblioteca PyTorch Lightning, así que siempre puedes fijarte en [su documentación](https://pytorch-lightning.readthedocs.io/en/latest/loggers.html#weights-and-biases).

## ⚡ Yendo a la velocidad de la luz con solo dos líneas:

```python
from pytorch_lightning.loggers import WandbLogger
from pytorch_lightning import Trainer

wandb_logger = WandbLogger()
trainer = Trainer(logger=wandb_logger)
```

## ✅ ¡Comprueba ejemplos reales!

Hemos creado algunos ejemplos para que veas cómo funciona la integración:

*  [Demostración en Google Colab](https://colab.research.google.com/drive/16d1uctGaw2y9KhGBlINNTsWpmlXdJwRW?usp=sharing) con optimización de hiperparámetros
*  [Tutorial](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch-lightning/Supercharge_your_Training_with_Pytorch_Lightning_%2B_Weights_%26_Biases.ipynb): Sobrecarga tu Entrenamiento con PyTorch Lightning + Weights & Biases
*  [Segmentación Semántica con Lightning](https://app.wandb.ai/borisd13/lightning-kitti/reports/Lightning-Kitti--Vmlldzo3MTcyMw): optimiza redes neuronales para vehículos autónomos
*  [Una guía paso a paso](https://app.wandb.ai/cayush/pytorchlightning/reports/Use-Pytorch-Lightning-with-Weights-%26-Biases--Vmlldzo2NjQ1Mw) para hacer el seguimiento del desempeño de tu modelo de Lightning

## **💻** Referencia de la API

### `WandbLogger`

 Parámetros Optativos:

* **name \(str\)** – visualiza el nombre para la ejecución.
* **save\_dir** \(_str_\) – ruta donde son guardados los datos \(por defecto, el directorio wandb\).
* **offline** \(_bool_\) – ejecuta fuera de línea \(los datos pueden ser transmitidos más tarde a los servidores de wandb\).
* **id** \(_str_\) – establece la versión, principalmente usado para reanudar una ejecución previa.
* **version** \(_str_\) – igual que version \(legacy\).
* **anonymous** \(_bool_\) – habilita o deshabilita explícitamente el registro anónimo.
* **project** \(_str_\) – el nombre del proyecto al que pertenece esta ejecución.
* **log\_model** \(_bool_\) – guarda puntos de control en el directorio wandb para subir a los servidores de W&B.
* **prefix** \(_str_\) – string para poner al comienzo de las claves de las métricas.
* **sync\_step** \(_bool_\) - sincroniza el paso del Entrenador con el paso de wandb \(por defecto es True\).
* **\*\*kwargs** –  Los argumentos adicionales como `entity`, `group`, `tags`, etc., usados por `wandb.init`, pueden ser pasados como argumentos de palabras claves en este registrador.

### **`WandbLogger.watch`**

Registra la topología del modelo, así también como, de forma opcional, los gradientes y los pesos.

```python
wandb_logger.watch(model, log='gradients', log_freq=100)
```

Parámetros:

* **model** \(_nn.Module_\) – modelo que se va a registrar.
* **log** \(_str_\) – puede ser “gradients” \(por defecto\), “parameters”, “all” o None.
* **log\_freq** \(_int_\) – cuenta de pasos entre el registro de los gradientes y los parámetros \(por defecto es 100\)

### **`WandbLogger.log_hyperparams`**

Registra la configuración de los hiperparámetros.

Nota: esta función es llamada automáticamente cuando se usa LightningModule.save\_hyperparameters\(\)_`LightningModule.save_hyperparameters()`_

```python
wandb_logger.log_hyperparams(params)
```

Parámetros

* **params** \(dict\)  – diccionario con los nombres de los hiperparámetros como claves, y los valores de configuración como valores

### `WandbLogger.log_metrics`

Registra métricas de entrenamiento.

Nota: esta función es llamada automáticamente por `LightningModule.log('metric', value)`

```python
wandb_logger.log_metrics(metrics, step=None)
```

Parámetros:

* **metric \(numeric\)** – diccionario con nombres de métricas como claves y cantidades medidas como valores
* **step \(int\|None\)** – número de paso en el que las métricas deberían registrarse

\*\*\*\*

