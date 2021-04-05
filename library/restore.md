---
description: >-
  Restaurer un fichier, comme un checkpoint de modèle, dans votre fichier local
  d’essai pour y accéder dans votre script
---

# wandb.restore\(\)

##  Aperçu

 Appeler `wandb.restore(nomdefichier)` restaurera un fichier dans votre dossier local d’essai. Typiquement, `nomdefichier` renvoie à un fichier généré par un essai antérieur qui a été sauvegardé sur notre cloud. Cet appel créera une copie locale de ce fichier et retournera un stream ouvert à la lecture de fichier local.

`wandb.restore` accepte quelques arguments clefs optionnels :

* **run\_path** — chaîne de données qui réfère à l’essai précédent duquel le fichier est extrait, formatté sous forme $NOM\_D\_ENTITE/$NOM\_DU\_PROJET/$ID\_DU\_RUN' ou _'$NOM\_DU\_PROJET/$ID\_DU\_RUN' \(par défaut, entité en cours, nom du projet, et ID du run\)_
* **replace** —  ****booléen qui spécifie s’il faut écrire par-dessus une copie locale du nom de fichier avec la copie du cloud, si une copie locale se trouve être disponible \(par défaut : False\)
* **root** — chaîne de données qui spécifie le dossier dans lequel stocker la copie locale du fichier. Par défaut, le dossier de travail en cours, ou le `wandb.run.dir` si wandb.init a été appelé plus tôt \(par défaut : "."\)

Utilisations fréquentes :

* Restaurer l’architecture ou les poids d’un modèle générés dans des essais précédents
*   Reprendre l’entraînement depuis le dernier checkpoint, au cas-où une erreur serait arrivée \(voir la section [Reprise](https://docs.wandb.ai/library/resuming) pour certains détails importants\) 

## Exemples

Voir [ce rapport](https://app.wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W%26B--Vmlldzo3MDQ3Mw) pour voir un exemple complet de travail.

```python
# restore a model file from a specific run by user "vanpelt" in "my-project"
best_model = wandb.restore('model-best.h5', run_path="vanpelt/my-project/a1b2c3d")

# restore a weights file from a checkpoint
# (NOTE: resuming must be configured if run_path is not provided)
weights_file = wandb.restore('weights.h5')
# use the "name" attribute of the returned object
# if your framework expects a filename, e.g. as in Keras
my_predefined_model.load_weights(weights_file.name)
```

> Si vous ne spécifiez pas de run\_path, vous aurez besoin de configurer la [reprise](https://docs.wandb.ai/library/resuming) de votre essai. 
>
> Si vous voulez accéder de manière programmable à des fichiers en dehors de l’entraînement, utilisez l’[API Run](https://docs.wandb.ai/library/restore).

