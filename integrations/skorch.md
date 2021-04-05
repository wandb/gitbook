# Skorch

Puedes utilizar Weights & Biases con Skorch para registrar automáticamente al modelo con el mejor desempeño – junto con todas las métricas de desempeño del modelo, la topología del mismo y los recursos computacionales después de cada época. Cada archivo guardado en wandb\_run.dir es registrado automáticamente a los servidor de W&B.

Ver [ejecución de ejemplo](https://app.wandb.ai/borisd13/skorch/runs/s20or4ct?workspace=user-borisd13)

## Parámetros

<table>
  <thead>
    <tr>
      <th style="text-align:left">Par&#xE1;metro</th>
      <th style="text-align:left">Descripci&#xF3;n</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>wandb_run</b>:</p>
        <p>wandb.wandb_run.Run</p>
      </td>
      <td style="text-align:left">Ejecuci&#xF3;n de wandb usada para registrar datos.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>save_model<br /></b>bool (default=True)</td>
      <td style="text-align:left">Para guardar un punto de control del mejor modelo y subirlo a tu Ejecuci&#xF3;n
        en los servidores de W&amp;B.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>keys_ignored<br /></b>str or list of str (default=None)</td>
      <td style="text-align:left">Clave o lista de claves que no deber&#xED;an ser registradas en tensorboard.
        Ten en cuenta que en adici&#xF3;n a las claves provistas por el usuario,
        aquellas claves como las que comienzan con &#x2018;event_&#x2019; o como
        las que terminan con &#x2018;_best&#x2019;, por defecto son ignoradas.</td>
    </tr>
  </tbody>
</table>

## Código de ejemplo

Hemos creado algunos ejemplos para que veas cómo funciona la integración

* [Colab](https://colab.research.google.com/drive/1Bo8SqN1wNPMKv5Bn9NjwGecBxzFlaNZn?usp=sharing): Una demostración simple para probar la integración
* [Una guía paso a paso](https://app.wandb.ai/cayush/uncategorized/reports/Automate-Kaggle-model-training-with-Skorch-and-W%26B--Vmlldzo4NTQ1NQ): para hacer el seguimiento del desempeño de tu modelo de Skorch

```python
# Install wandb
... pip install wandb

import wandb
from skorch.callbacks import WandbLogger

# Create a wandb Run
wandb_run = wandb.init()
# Alternative: Create a wandb Run without a W&B account
wandb_run = wandb.init(anonymous="allow")

# Log hyper-parameters (optional)
wandb_run.config.update({"learning rate": 1e-3, "batch size": 32})

net = NeuralNet(..., callbacks=[WandbLogger(wandb_run)])
net.fit(X, y)
```

## Métodos

| Método | Descripción |
| :--- | :--- |
| `initialize`\(\) | \(Re\)Establece el estado inicial del callback. |
| `on_batch_begin`\(net\[, X, y, training\]\) | Llamado al comienzo de cada lote. |
| `on_batch_end`\(net\[, X, y, training\]\) | Llamado al final de cada lote. |
| `on_epoch_begin`\(net\[, dataset\_train, …\]\) | Llamado al comienzo de cada época. |
| `on_epoch_end`\(net, \*\*kwargs\) | Registra valores desde el último paso del historial y guarda el mejor modelo. |
| `on_grad_computed`\(net, named\_parameters\[, X, …\]\) | Es llamado una vez por lote, después de que los gradientes hayan sido computados, pero antes de que haya sido ejecutado un paso de actualización. |
| `on_train_begin`\(net, \*\*kwargs\) | Registra la topología del modelo y agrega un enganche para los gradientes. |
| `on_train_end`\(net\[, X, y\]\) | Llamado al final del entrenamiento. |

