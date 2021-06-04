# Skorch

Vous pouvez utiliser Weights & Biases avec Skorch pour enregistrer automatiquement le modèle ayant la meilleure performance – ainsi que les métriques de performance de tous les modèles, la topologie et les ressources de calcul du modèle après chaque epoch. Chaque fichier sauvegardé dans wandb\_run.dir est automatiquement enregistré sur les serveurs W&B.

Voir un [exemple d’essai](https://app.wandb.ai/borisd13/skorch/runs/s20or4ct?workspace=user-borisd13).

### Paramètres

<table>
  <thead>
    <tr>
      <th style="text-align:left">Param&#xE8;tre</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>wandb_run</b>:</p>
        <p>wandb.wandb_run.Run</p>
      </td>
      <td style="text-align:left">L&#x2019;ex&#xE9;cution wandb utilis&#xE9;e pour enregistrer les donn&#xE9;es.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>save_model<br /></b>bool (default=True)</td>
      <td style="text-align:left">S&#x2019;il faut ou non enregistrer un checkpoint du meilleur mod&#xE8;le
        et l&#x2019;envoyer &#xE0; votre Run sur les serveurs W&amp;B.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>keys_ignored<br /></b>str or list of str (default=None)</td>
      <td style="text-align:left">Clef ou liste de clefs qui ne doivent pas &#xEA;tre enregistr&#xE9;es
        dans le tensorboard. Notez qu&#x2019;en plus des clefs fournies par l&#x2019;utilisateur,
        les clefs comme celles qui commencent par &#x2018;event_&#x2019; ou qui
        finissent par &#x2018;_best&#x2019; seront ignor&#xE9;es par d&#xE9;faut.</td>
    </tr>
  </tbody>
</table>

##  Exemple de code

Nous avons préparé quelques exemples pour que vous puissiez voir comment fonctionne cette intégration :

* [**Colab**](https://colab.research.google.com/drive/1Bo8SqN1wNPMKv5Bn9NjwGecBxzFlaNZn?usp=sharing)**:** une démo simple pour essayer l’intégration
* \*\*\*\*[**Un guide pas-à-pas** ](https://app.wandb.ai/cayush/uncategorized/reports/Automate-Kaggle-model-training-with-Skorch-and-W%26B--Vmlldzo4NTQ1NQ)**:** pour suivre les évolutions de performance de votre modèle Skorch

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

##  Méthode

| Méthode | Description |
| :--- | :--- |
| `initialize`\(\) | \(Re-\)Paramètre l’état initial du callback. |
| `on_batch_begin`\(net\[, X, y, training\]\) | Appelé au début de chaque lot \(batch\). |
| `on_batch_end`\(net\[, X, y, training\]\) | Appelé à la fin de chaque lot \(batch\). |
| `on_epoch_begin`\(net\[, dataset\_train, …\]\) | Appelé au début de chaque epoch. |
| `on_epoch_end`\(net, \*\*kwargs\) | Enregistre les valeurs depuis la dernière étape d’historique et sauvegarde le meilleur modèle |
| `on_grad_computed`\(net, named\_parameters\[, X, …\]\) | Appelé une fois par lot après le calcul des dégradés mais avant qu’une étape d’update n’ait été effectuée. |
| `on_train_begin`\(net, \*\*kwargs\) | Enregistre la topologie du modèle et ajoute un crochet \(hook\) pour les dégradés |
| `on_train_end`\(net\[, X, y\]\) | Appelé à la fin de l’entraînement. |

