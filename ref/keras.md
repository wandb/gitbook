---
description: wandb.keras
---

# Keras Reference

[source](https://github.com/wandb/client/blob/master/wandb/keras/__init__.py#L148)

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

WandbCallback intègre automatiquement keras avec wandb.

**Exemples :**

```python
model.fit(X_train, y_train,  validation_data=(X_test, y_test),
callbacks=[WandbCallback()])
```

WandbCallback enregistrera automatiquement les données d’historique de toutes les mesures collectées par keras : pertes et tout ce qui est passé dans keras\_model.compile\(\)

WandbCallback paramètrera les mesures de sommaire pour l’essai associé avec la "meilleure" étape d’entraînement, où "meilleure" \(best\) est définie par les attributs `monitor` et `mode`. Par défaut, c’est l’epoch avec la val\_loss la plus basse. WandbCallback sauvegardera par défaut le modèle associé avec la meilleure epoch.

WandbCallback peut optionnellement enregistrer des histogrammes de dégradés et de paramètres.

WandbCallback peut optionnellement sauvegarder les données d’entraînement et de validation pour que wandb en fasse des visuels.

**Arguments**:

* `monitor` _str_ - nom de mesure à surveiller. Par défaut, val\_loss.
* `mode` _str_ -un de {"auto", "min", "max"}. "min" – sauvegarde modèle lorsque monitor est minimal "max" – sauvegarde modèle lorsque monitor est maximal "auto" – essaye de deviner quand sauvegarder modèle \(par défaut\).
* `save_weights_only` si True, sauvegarde uniquement les poids du modèle`model.save_weights(filepath)`\), sinon le modèle entier \(`model.save(filepath)`\).
* `log_weights` - \(booléen\) si True, sauvegarde histogrammes des poids de couches du modèle.
* `log_gradients` -  \(booléen\) si True, enregistre histogrammes des dégradés d’entraînement. Le modèle doit définir une `total_loss (perte totale)`.
* `training_data` - \(tuple\) Même format \(X,y\) que passé dans model.fit. Nécessaire pour calculer les dégradés – obligatoire si `log_gradients` est `True`.
* `validation_data` - \(tuple\) Même format \(X,y\) que passé dans model.fit. Un set de données à visualiser par wandb. Si paramétré, à chaque epoch, wandb fera un petit nombre de prédictions et sauvegardera les résultats pour visualisation future.
* `generator` _générateur_ – un générateur qui renvoie les données de visualisation à visualiser par wandb. Ce générateur devrait renvoyer des tuples \(X,y\). Soit validate\_data soit generator doit être paramétré pour que wandb visualise des exemples de données spécifiques.
* `validation_steps` _int_ - si validation\_data est un generator, nombre d’étapes sur lesquelles exécuter le générateur pour la validation complète du set.
* `labels` _list_ - Si vous visualisez des données avec wandb, cette liste de labels convertira les output numériques en chaînes compréhensibles si vous construisez un classifieur multi-classes. Si vous constituez un classifieur binaire, vous pouvez passer une liste de deux labels \["label for false", "label for true"\]. Si validate\_data et generator sont tous deux False, ne fera rien.
* `predictions` _int_ – le nombre de prédictions à faire pour visualisation à chaque epoch, max de 100.
* `input_type` _string_ - _haîne_ – type d’input du modèle pour aider à la visualisation. peut être un de : \("image", "images", "segmentation\_mask"\).
* `output_type` _haîne_ – type d’input du modèle pour aider à la visualisation. peut être un de : \("image", "images", "segmentation\_mask"\).
* `log_evaluation` _booléen_ – si True, sauvegarde une dataframe qui contient les résultats de validation complète à la fin de l’entraînement.
* `class_colors` _\[float, float, float\]_ – si l’input ou l’output est un masque de segmentation, un array qui contient un tuple rgb \(range 0-1\) pour chaque classe.
* `log_batch_frequency` _int_ – si None, le callback enregistrera chaque epoch. Si réglé sur integer, le callback enregistrera les mesures d’entraînement à chaque lots log\_batch\_frequency.
* `log_best_prefix` _chaîne – si None, aucune mesure de sommaire supplémentaire ne sera enregistrée. Si réglé sur une chaîne, la mesure et l’epoch surveillées seront précédées de cette valeur et stockées en tant que mesures de sommaire._

