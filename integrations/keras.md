---
description: How to integrate a Keras script to log metrics to W&B
---

# Keras

Utilisez le callback Keras pour sauvegarder automatiquement toutes les mesures et toutes les valeurs de perte retracées dans `model.fit` .

{% code title="example.py" %}
```python
import wandb
from wandb.keras import WandbCallback
wandb.init(config={"hyper": "parameter"})

# Magic

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
          callbacks=[WandbCallback()])
```
{% endcode %}

 Essayez notre intégration dans un [notebook colab](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/keras/Simple_Keras_Integration.ipynb), complété par un [tutoriel vidéo](https://www.youtube.com/watch?v=Bsudo7jbMow&ab_channel=Weights%26Biases), ou regardez nos [exemples de projets](https://docs.wandb.ai/examples) pour avoir un exemple de script complet.  


#### Options

La classe Keras `WandbCallback()` permet un certain nombre d’options :

| Argument mot-clef | Par défaut | Description |
| :--- | :--- | :--- |
| monitor | val\_loss | Mesure d’entraînement utilisée pour mesurer les performances pour sauvegarder le meilleur modèle. i. e. val\_loss |
| mode | auto |  'min', 'max', ou 'auto' : comment comparer la mesure d’entraînement spécifiée dans `monitor` entre les étapes |
| save\_weights\_only | False | ne sauvegarde que les poids plutôt que le modèle intégral |
| save\_model | True | sauvegarde le modèle s’il s’est amélioré à chaque étape |
| log\_weights | False | enregistre les valeurs de chaque paramètres de couches à chaque epoch |
| log\_gradients | False | enregistre les dégradés de chaque paramètres de couches à chaque epoch |
| training\_data | None | tuple \(X, y\) nécessaire pour calculer les dégradés |
| data\_type | None | le type de données que l’on sauvegarde, pour l’instant seul "image" est accepté |
| labels | None | utilisé seulement si data\_type est spécifié ; liste de labels sur laquelle convertir des sorties numériques si vous construisez une classification. \(accepte une classification binaire\) |
| predictions | 36 | le nombre de prédictions à faire si le data\_type est spécifié. Max de 100. |
| generator | None | si vous utilisez de l’augmentation de données et un data\_type, vous pouvez spécifier un générateur avec lequel faire des prédictions |

## Questions fréquentes

### Utiliser le multiprocessing Keras avec wandb

Si vous paramètrez `use_multiprocessing=True`et que vous voyez l’erreur`Error('You must call wandb.init() before wandb.config.batch_size')` essayez ceci :

1. Dans l’init Sequence class, ajoutez : `wandb.init(group='...')` 
2. Dans votre programme principal, assurez-vous que vous utilisez  `if __name__ == "__main__":` puis ajouter le reste de votre logique de script à l’intérieur de ça.

## Exemples

Nous avons créé quelques exemples pour que vous voyiez comment cette intégration fonctionne :

*  [Exemple sur Github ](https://github.com/wandb/examples/blob/master/examples/keras/keras-cnn-fashion/train.py): Exemple Fashion MNIST dans un script Python
* Exécutez dans Google Colab : Un exemple notebook simple pour bien commencer
* [Tableau de bord Wandb ](https://app.wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs): Visualiser les résultats sur W&B

