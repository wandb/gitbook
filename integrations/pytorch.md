---
description: How to integrate a PyTorch script to log metrics to W&B
---

# PyTorch

W&B provee soporte de primera clase para PyTorch. Para registrar gradientes y almacenar la topología de la red de forma automática, puedes llamar a watch, y pasarle tu modelo de PyTorch.

```python
import wandb
wandb.init(config=args)

# Magic
wandb.watch(model)

model.train()
for batch_idx, (data, target) in enumerate(train_loader):
    output = model(data)
    loss = F.nll_loss(output, target)
    loss.backward()
    optimizer.step()
    if batch_idx % args.log_interval == 0:
        wandb.log({"loss": loss})
```

> Los gradientes, las métricas y el gráfico no van a ser registrados hasta que se llame a `wandb.log`, después de una pasada hacia adelante y una hacia atrás.

Mira esta [notebook ](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)[de ](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb)[colab](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb) para ver un ejemplo de punta a punta de la integración de wandb con Pytorch, incluyendo un [video tutorial](https://www.youtube.com/watch?v=G7GH0SeNBMA&ab_channel=Weights%26Biases). También puedes encontrar más ejemplos en nuestra sección de [proyectos de ejemplo](https://docs.wandb.ai/examples).

### Opciones

 Por defecto, el enganche sólo registra gradientes.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Argumentos</th>
      <th style="text-align:left">Opciones</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">
        <ul>
          <li>all: registra histogramas de gradientes y par&#xE1;metros</li>
          <li>gradients (por defecto)</li>
          <li>parameters (pesos del modelo)</li>
          <li>None</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">log_freq</td>
      <td style="text-align:left">integer (1000 por defecto): El n&#xFA;mero de pasos entre los gradientes
        del registro</td>
    </tr>
  </tbody>
</table>

## Imágenes

Puedes pasar tensores de PyTorch con los datos de las imágenes a `wandb.Image` y torchvision utils va a ser usado para registrarlos automáticamente.

Para registrar imágenes y verlas en el panel Media, puedes usar la siguiente sintaxis:

```python
wandb.log({"examples" : [wandb.Image(i) for i in images]})
```

## Múltiples Modelos

Si necesitas hacer el seguimiento de múltiples modelos en el mismo script, puedes llamar \(wall??\) a wandb.watch\(\) en cada modelo de forma separada.

## Ejemplo

Hemos creado algunos ejemplos para que veas cómo funciona la integración:

* [Ejecución en Google Colab](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb): un ejemplo de una notebook simple para empezar
* [Ejemplo en Github](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-mnist/main.py): Ejemplo MNIST en un script de Python
* [Tablero de control de wandb](https://app.wandb.ai/wandb/pytorch-mnist/runs/): Mira los resultados en W&B

