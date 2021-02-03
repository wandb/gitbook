# Fast.ai

Si estás usando fastai para entrenar tus modelos, W&B permite una integración fácil utilizando el WandbCallback. Explora los detalles en la [documentación interactiva con los ejemplos →](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)

Primero instala Weights & Biases e inicia sesión:

```text
pip install wandb
wandb login
```

 Entonces agrega el callback al método `learner` o al `método fit`:

```python
import wandb
from fastai.callback.wandb import *

# start logging a wandb run
wandb.init(project='my_project')

# To log only during one training phase
learn.fit(..., cbs=WandbCallback())

# To log continuously for all training phases
learn = learner(..., cbs=WandbCallback())
```

{% hint style="info" %}
Si usas la versión 1 de Fastai, consulta la [documentación de Fastai v1](https://docs.wandb.ai/integrations/fastai/v1).
{% endhint %}

`WandbCallback` acepta los siguientes argumentos:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Argumentos</th>
      <th style="text-align:left">Descripci&#xF3;n</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">&#x201C;gradients&#x201D; (por defecto), &#x201C;parameters&#x201D;, &#x201C;all&#x201D;
        o None. Las p&#xE9;rdidas y las m&#xE9;tricas siempre son registradas.</td>
    </tr>
    <tr>
      <td style="text-align:left">log_preds</td>
      <td style="text-align:left">si quieres registrar las muestras de las predicciones (por defecto es
        True).</td>
    </tr>
    <tr>
      <td style="text-align:left">log_model</td>
      <td style="text-align:left">si quieres registrar nuestro modelo (por defecto es True). Esto tambi&#xE9;n
        requiere SaveModelCallback.</td>
    </tr>
    <tr>
      <td style="text-align:left">log_dataset</td>
      <td style="text-align:left">
        <ul>
          <li>False (por defecto)</li>
          <li>True registrar&#xE1; el directorio referenciado por learn.dls.path.</li>
          <li>puede ser definida expl&#xED;citamente una ruta para referenciar qu&#xE9;
            directorio registrar.</li>
        </ul>
        <p>Nota: el subdirectorio &#x201C;models&#x201D; siempre es ignorado.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">dataset_name</td>
      <td style="text-align:left">nombre del conjunto de datos registrado (por defecto, el nombre del directorio).</td>
    </tr>
    <tr>
      <td style="text-align:left">valid_dl</td>
      <td style="text-align:left"><code>DataLoaders</code> conteniendo elementos utilizados por las muestras
        de la predicci&#xF3;n (por defecto, a los elementos aleatorios de <code>learn.dls.valid</code>).</td>
    </tr>
    <tr>
      <td style="text-align:left">n_preds</td>
      <td style="text-align:left">n&#xFA;mero de predicciones registradas (por defecto, a 36).</td>
    </tr>
    <tr>
      <td style="text-align:left">seed</td>
      <td style="text-align:left">usado para definir muestras aleatorias.</td>
    </tr>
  </tbody>
</table>

Para procesos de trabajos personalizados, puedes registrar tus conjuntos de datos y modelos de forma manual:

* `log_dataset(path, name=None, medata={})`
* `log_model(path, name=None, metadata={})` 

 Nota: cualquier subdirectorio “models” va a ser ignorado.

## Ejemplos

*  [Visualiza, haz un seguimiento, y compara ](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)[los](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)[ modelos de Fastai](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA): Una guía completamente documentada
* [Segmentación de Imágenes en CamVid](http://bit.ly/fastai-wandb): Un caso de uso de ejemplo de la integración

