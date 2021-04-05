# History

## wandb.sdk.wandb\_history

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_history.py#L3)

 L’historique garde une trace des données enregistrées au fil du temps. Pour utiliser l’historique de votre script, appelez wandb.log {"key": value}\) à une seule ou plusieurs étapes dans le temps dans votre boucle d’entraînement. Cela génère une série de temps de scalaires ou de médias sauvegardés dans l’historique.

Dans l’IU, si vous enregistrez un scalaire à plusieurs étapes dans le temps, W&B tracera ces mesures d’historiques sous forme de graphique linéaire par défaut. Si vous enregistrez une seule valeur d’historique, vous pouvez la comparer à travers tous vos essais avec un graphique en barre.

Il est souvent pratique de retracer une série chronologie complète, ainsi qu’une valeur unique de sommaire. Par exemple, précision à chaque étape dans Historique et meilleure précision dans Sommaire. Par défaut, Sommaire est paramétré sur la valeur finale d’Historique.

### Objets History

```python
class History(object)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_history.py#L23)

Données de série chronologique. Essentiellement, une liste de dicts où chaque dict est un set de statistiques de sommaire enregistrées.

