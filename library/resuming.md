# Resuming

 Vous pouvez faire en sorte que wandb reprenne automatiquement vos essais en passant resume=True dans wandb.init\(\). Si votre processus ne s’est pas fermé avec succès, la prochaine fois que vous le lancerez, wandb reprendra l’enregistrement depuis la dernière étape. Ci-dessous, un exemple simple dans Keras :

```python
import keras
import numpy as np
import wandb
from wandb.keras import WandbCallback
wandb.init(project="preemptable", resume=True)

if wandb.run.resumed:
    # restore the best model
    model = keras.models.load_model(wandb.restore("model-best.h5").name)
else:
    a = keras.layers.Input(shape=(32,))
    b = keras.layers.Dense(10)(a)
    model = keras.models.Model(input=a,output=b)

model.compile("adam", loss="mse")
model.fit(np.random.rand(100, 32), np.random.rand(100, 10),
    # set the resumed epoch
    initial_epoch=wandb.run.step, epochs=300,
    # save the best model if it improved each epoch
    callbacks=[WandbCallback(save_model=True, monitor="loss")])
```

La reprise automatique fonctionne uniquement si le processus recommence depuis le haut du même fichier système que le processus qui a échoué. Si vous ne pouvez pas partager un fichier système, nous vous permettons de régler **WANDB\_RUN\_ID** : une chaîne globalement unique \(par projet\) qui correspond à un seul essai de votre script. Elle ne doit pas excéder 64 caractères. Tous les caractères non-reconnus seront transformés en tiret.

```python
# store this id to use it later when resuming
id = wandb.util.generate_id()
wandb.init(id=id, resume="allow")
# or via environment variables
os.environ["WANDB_RESUME"] = "allow"
os.environ["WANDB_RUN_ID"] = wandb.util.generate_id()
wandb.init()
```

Si vous réglez **WANDB\_RESUME** égale à "allow" \(permettre\), vous pouvez toujours régler **WANDB\_RUN\_ID** sur une chaîne unique, et les reprises depuis le début du processus seront automatiquement gérées. Si vous réglez **WANDB\_RESUME** égale à "must" \(devoir\), wandb vous enverra une erreur si l’essai à reprendre n’existe pas encore, plutôt que de créer automatiquement un nouvel essai.

| Méthode | Syntaxe | Jamais reprendre \(défaut\) | Toujours reprendre | Reprendre en spécifiant un ID d’essai \(run\_id\) | Reprendre depuis le même dossier |
| :--- | :--- | :--- | :--- | :--- | :--- |
| ligne de commande | wandb run --resume= | "never" | "must" | "allow" \(Requires WANDB\_RUN\_ID=RUN\_ID\) | \(not available\) |
| environnement | WANDB\_RESUME= | "never" | "must" | "allow" \(Requires WANDB\_RUN\_ID=RUN\_ID\) | \(not available\) |
| init | wandb.init\(resume=\) |  | \(not available\) | resume=RUN\_ID | resume=True |

{% hint style="warning" %}
Si plusieurs processus utilisent le même run\_id de manière concurrente, des résultats inattendus seront enregistrés et une limite de taux interviendra.
{% endhint %}

{% hint style="info" %}
Si vous reprenez un essai et que vous avez des **notes** spécifiées dans wand.init\(\) , ces notes écraseront toutes notes que vous avez ajoutées dans l’IU.
{% endhint %}

