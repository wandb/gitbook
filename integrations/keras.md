---
description: How to integrate a Keras script to log metrics to W&B
---

# Keras

Utilisez la fonction de rappel \(callback\) de Keras pour sauvegarder automatiquement tous les métriques et toutes les valeurs de perte retracées dans `model.fit` .

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

 Essayez notre intégration dans ce [notebook colab](https://colab.research.google.com/github/wandb/examples/blob/master/colabs/keras/Simple_Keras_Integration.ipynb), fourni avec un [tutoriel vidéo](https://www.youtube.com/watch?v=Bsudo7jbMow&ab_channel=Weights%26Biases), ou consultez nos[ exemples de projets ](https://docs.wandb.ai/v/fr/examples)pour avoir un exemple de script complet.   


#### Options

La classe Keras `WandbCallback()` permet un certain nombre d’options :

| Argument mot-clef | Par défaut | Description |
| :--- | :--- | :--- |
| monitor | val\_loss | Métrique d’entraînement utilisée pour mesurer les performances pour sauvegarder le meilleur modèle. i. e. val\_loss |
| mode | auto | 'min', 'max', ou 'auto' : comment comparer le métrique d’entraînement spécifié dans `monitor` entre les étapes |
| save\_weights\_only | False | ne sauvegarde que les poids au lieu de l’intégralité du modèle |
| save\_model | True | sauvegarde le modèle s’il s’est amélioré à chaque étape |
| log\_weights | False | enregistre les valeurs de chaque paramètres de couches à chaque epoch |
| log\_gradients | False | enregistre les dégradés de chaque paramètres de couches à chaque epoch |
| training\_data | None | tuple \(X, y\) nécessaire pour calculer les dégradés |
| data\_type | None | le type de données que nous sauvegardons, pour l’instant seules les images sont acceptées |
| labels | None | utilisé uniquement si data\_type est spécifié ; liste d’étiquettes pour convertir des sorties numériques si vous développez une classification \(accepte une classification binaire\). |
| predictions | 36 | le nombre de prédictions à faire si le data\_type est spécifié \(100 max.\). |
| generator | None | si vous utilisez une augmentation de données et un data\_type, vous pouvez spécifier un générateur de prédictions |

## Questions fréquentes

### Utiliser le multiprocessing Keras avec wandb

Si vous configurez `use_multiprocessing=Tru`e et que vous voyez le message d’erreur `Error('You must call wandb.init() before wandb.config.batch_size')`, essayez ceci :

1. Dans l’init Sequence class, ajoutez : `wandb.init(group='...')` 
2. Dans votre programme principal, assurez-vous que vous utilisez  `if __name__ == "__main__":` puis insérez-y le reste de votre logique de script.

## Exemples

Nous avons créé quelques exemples pour vous montrer comment cette intégration fonctionne :

*  [Exemple sur Github ](https://github.com/wandb/examples/blob/master/examples/keras/keras-cnn-fashion/train.py): exemple Fashion MNIST dans un script Python
* Exécutez dans Google Colab : un simple exemple de notebook pour bien commencer
* [Tableau de bord Wandb ](https://app.wandb.ai/wandb/keras-fashion-mnist/runs/5z1d85qs): visualiser les résultats sur W&B

