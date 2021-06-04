# Fast.ai

Si vous utilisez **fastai** pour entraîner vos modèles, W&B propose une intégration simplifiée en utilisant le WandbCallback. Retrouvez tous les détails dans notre [documentation interactive avec des exemples →](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA)​

Tout d’abord, installez Weights & Biases et connectez-vous :

```text
pip install wandb
wandb login
```

 Puis, ajoutez la fonction de rappel \(callback\) à la méthode `learner` ou `fit` :

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
 \(i\) Si vous utilisez la version 1 de Fastai, référez-vous à la [documentation Fastai v1.](https://docs.wandb.ai/integrations/fastai/v1)  
{% endhint %}

`WandbCallback` accepte les arguments suivants :

<table>
  <thead>
    <tr>
      <th style="text-align:left">Arguments</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">&quot;gradients&quot; (par d&#xE9;faut), &quot;parameters&quot;, &quot;all&quot;
        ou Aucun. Les pertes et les mesures sont toujours enregistr&#xE9;es.</td>
    </tr>
    <tr>
      <td style="text-align:left">log_preds</td>
      <td style="text-align:left">si vous voulez ou non enregistrer les &#xE9;chantillons de pr&#xE9;dictions
        (par d&#xE9;faut, True).</td>
    </tr>
    <tr>
      <td style="text-align:left">log_model</td>
      <td style="text-align:left">si vous voulez ou non enregistrer votre mod&#xE8;le (par d&#xE9;faut,
        True). Requiert &#xE9;galement SaveModelCallback.</td>
    </tr>
    <tr>
      <td style="text-align:left">log_dataset</td>
      <td style="text-align:left">
        <ul>
          <li>False (par d&#xE9;faut)</li>
          <li>True enregistrera un dossier r&#xE9;f&#xE9;renc&#xE9; par learn.dls.path.</li>
          <li>Un chemin d&#x2019;acc&#xE8;s peut &#xEA;tre explicitement d&#xE9;fini
            pour r&#xE9;f&#xE9;rencer dans quel dossier enregistrer.</li>
        </ul>
        <p><em>Note : </em>Tout sous-dossier &quot;models&quot; sera syst&#xE9;matiquement
          ignor&#xE9;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">dataset_name</td>
      <td style="text-align:left">Nom du dataset enregistr&#xE9; (par d&#xE9;faut, nom du dossie</td>
    </tr>
    <tr>
      <td style="text-align:left">valid_dl</td>
      <td style="text-align:left"><code>DataLoaders</code> contenant des objets (items) utilis&#xE9;s pour
        les &#xE9;chantillons de pr&#xE9;diction (par d&#xE9;faut, des objets al&#xE9;atoires
        issus de<code> learn.dls.valid</code> .)</td>
    </tr>
    <tr>
      <td style="text-align:left">n_preds</td>
      <td style="text-align:left">nombre de pr&#xE9;dictions enregistr&#xE9;es (par d&#xE9;faut, 36).</td>
    </tr>
    <tr>
      <td style="text-align:left">seed</td>
      <td style="text-align:left">utilis&#xE9;e pour d&#xE9;finir les &#xE9;chantillons al&#xE9;atoires.</td>
    </tr>
  </tbody>
</table>

Pour les flux de travaux personnalisés, vous pouvez manuellement enregistrer vos datasets et vos modèles :

* `log_dataset(path, name=None, medata={})`
* `log_model(path, name=None, metadata={})` 

Note : Tout sous-dossier "models" sera ignoré.

## Exemples

* [Visualisez, retracez et comparez des modèles Fastai ](https://app.wandb.ai/borisd13/demo_config/reports/Visualize-track-compare-Fastai-models--Vmlldzo4MzAyNA): un guide très documenté sur la marche à suivre
* [Segmentation d’images sur CamVid ](http://bit.ly/fastai-wandb): un exemple d’utilisation de l’intégration

