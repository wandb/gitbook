# Summary

## wandb.sdk.wandb\_summary

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L2)

### Objets SummaryDict

```python
@six.add_metaclass(abc.ABCMeta)
class SummaryDict(object)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L18)

Un dict-like qui enveloppe tous les dictionnaires imbriqués dans un SummarySubDict, et déclenche self.\_root.\_callback sur des changements de propriété.

### Summary Objects

```python
class Summary(SummaryDict)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L78)

Summary

Les statistiques de sommaire sont utilisées pour retracer les mesures uniques par modèle. Appeler wandb.log\({'accuracy': 0.9}\) paramètrera automatiquement wandb.summary\['accuracy'\] sur 0.9 à moins que le code ait manuellement changé wandb.summary\['accuracy'\].

Paramétrer manuellement wandb.summary\['accuracy'\] peut être utile si vous voulez avoir un enregistrement de la précision \(accuracy\) de votre meilleur modèle tout en utilisant wandb.log\(\) pour avoir un enregistrement de la précision à chaque étape.

Vous pouvez vouloir enregistrer des mesures d’évaluations dans un sommaire de runs après la fin de l’entraînement. Les sommaires prennent en charge les numpy arrays, les tenseurs pytorch et les tenseurs tensorflow. Lorsqu’une valeur est de l’un de ces formats, nous faisons persister tout le tenseur dans un fichier binaire et nous enregistrons des mesures de haut niveau dans l’objet de sommaire, telles que min, moyenne, variance, percentile 95%, etc.

 **Exemples :**

```text
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
test_loss, test_accuracy = test()
if (test_accuracy > best_accuracy):
wandb.run.summary["best_accuracy"] = test_accuracy
best_accuracy = test_accuracy
```

###  Objets SummarySubDict

```python
class SummarySubDict(SummaryDict)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_summary.py#L128)

Node non-root de la structure de données du sommaire. Contient un chemin vers lui-même depuis la root.

