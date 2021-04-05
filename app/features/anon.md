---
description: >-
  Enregistrer et visualiser des essais sans créer de compte, et fournir du code
  aux examinateurs de documents qu’ils puissent exécuter et visualise avoir
  besoin de mettre en place Weights & Biases.
---

# Anonymous Mode

Est-ce que vous écrivez un article à soumettre à une conférence ? Utilisez le **mode anonyme** pour permettre à n’importe qui d’exécuter votre code et obtenir un tableau de bord Weights & Biases sans créer de compte.

```python
wandb.init(anonymous="allow")
```

##  Exemple d’utilisation

Essayez [cet exemple de notebook](http://bit.ly/anon-mode) pour voir comment le mode anonyme fonctionne en pleine action.

```python
import wandb
import random

wandb.init(project="anon-demo", 
           anonymous="allow",
           config={
               "learning_rate": 0.1,
               "batch_size": 128,
           })

for step in range(30):
  wandb.log({
      "acc": random.random(),
      "loss": random.random()
  })
```

