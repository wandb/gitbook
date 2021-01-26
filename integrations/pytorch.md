---
description: How to integrate a PyTorch script to log metrics to W&B
---

# PyTorch

Pour automatiquement enregistrer des dégradés et stocker la topologie du réseau, vous pouvez appeler `watch` et passer votre modèle PyTorch.

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

> Les gradients, les mesures et le tableau ne seront pas enregistrés jusqu’à ce que `wandb.log` soit appelé après une première passe avant-arrière \(forward/backward\).

Consultez ce [notebook colab](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb) pour voir un exemple intégral d’intégration de wandb avec PyTorch, avec un [tutoriel vidéo](https://www.youtube.com/watch?v=G7GH0SeNBMA&ab_channel=Weights%26Biases). Vous pouvez aussi trouver d’autres exemples dans notre section [exemples de projets](https://docs.wandb.ai/examples).

### Options

Par défaut, le crochet \(hook\) n’enregistre que les dégradés \(gradients\).

<table>
  <thead>
    <tr>
      <th style="text-align:left">Arguments</th>
      <th style="text-align:left">Options</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">
        <ul>
          <li>all : enregistre les histogrammes de d&#xE9;grad&#xE9;s et de param&#xE8;tres</li>
          <li>gradients (default)</li>
          <li>parameters (poids du mod&#xE8;le)</li>
          <li>Aucun</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">log_freq</td>
      <td style="text-align:left">integer (default 1000): Le nombre d&#x2019;&#xE9;tapes entre l&#x2019;enregistrement
        de d&#xE9;grad&#xE9;s</td>
    </tr>
  </tbody>
</table>

## Images

Vous pouvez passer des tenseurs Pytorch avec des données d’images dans `wandb.Image` et des utils torchvision seront utilisées pour les enregistrer automatiquement.

Pour enregistrer des images et les visualiser dans le panneau Médias, vous pouvez utiliser la syntaxe suivante :

```python
wandb.log({"examples" : [wandb.Image(i) for i in images]})
```

## Modèles multiples

Si vous avez besoin de tracer plusieurs modèles dans le même script, vous pouvez inscrire wandb.watch\(\) sur chaque modèle de manière séparée.

##  Exemple

 Nous avons créé quelques exemples pour que vous puissiez voir comment l’intégration fonctionne :

*  [Exécuter dans Google Colab ](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/pytorch/Simple_PyTorch_Integration.ipynb): Un exemple simple de notebook pour bien commencer
*   [Exemple sur Github ](https://github.com/wandb/examples/blob/master/examples/pytorch/pytorch-cnn-mnist/main.py): Exemple MNIST dans un script Python
*   [Tableau de bord Wandb ](https://app.wandb.ai/wandb/pytorch-mnist/runs/): Visualiser des résultats sur W&B

