---
description: Sauvegarder un fichier dans le cloud pour associer l’essai en cours
---

# wandb.save\(\)

Il existe deux moyens de sauvegarder un fichier pour l’associer à un essai.

1. Utiliser `wandb.save(nomdufichier)` .
2. Placer un fichier dans le répertoire run wandb, qui sera téléchargé sur le cloud à la fin de l’essai.

{% hint style="info" %}
Si vous [reprenez](https://docs.wandb.ai/library/resuming) un essai précédent, vous pouvez retrouver votre fichier en appelant      wandb.restore\(nomdufichier\)
{% endhint %}

Si vous souhaitez synchroniser les fichiers pendant qu’ils sont en cours d’écriture, vous pouvez spécifier un nom de dossier ou un glob dans `wandb.save` .

##  Exemples de wandb.save

 Consultez [ce rapport](https://app.wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W%26B--Vmlldzo3MDQ3Mw) pour obtenir un exemple complet de travail.

```python
# Save a model file from the current directory
wandb.save('model.h5')

# Save all files that currently exist containing the substring "ckpt"
wandb.save('../logs/*ckpt*')

# Save any files starting with "checkpoint" as they're written to
wandb.save(os.path.join(wandb.run.dir, "checkpoint*"))
```

{% hint style="info" %}
Les dossiers d’essais locaux de W&B, par défaut, se trouvent dans le dossier ./wandb relatif à votre script, et le nom ressemble à run-20171023\_105053-3o4933r0, avec 20171023\_105053 les informations relatives à la date et l’heure, et 3o4933r0 l’ID de votre essai. Vous pouvez régler la variable d’environnement WANDB\_DIR ou l’argument clef dir de wandb.init sur un chemin absolu, et les fichiers seront alors écrits à l’intérieur de ce dossier.
{% endhint %}

## Exemple d’enregistrement d’un fichier dans le dossier wandb run

Le fichier "model.h5" est sauvegardé dans le wandb.run.dir et sera téléchargé sur le cloud à la fin de l’entraînement.

```python
import wandb
wandb.init()

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
    callbacks=[wandb.keras.WandbCallback()])
model.save(os.path.join(wandb.run.dir, "model.h5"))
```

 Voici un exemple de page publique. Vous pouvez voir sur les onglets du fichier qu’il y a le model-best.h5. C’est automatiquement sauvegardé par défaut par l’intégration Keras, mais vous pouvez sauvegarder un checkpoint manuellement et nous le stockerons pour vous en association avec votre essai.

[Voir l’exemple en direct →](https://app.wandb.ai/wandb/neurips-demo/runs/206aacqo/files)

![](../.gitbook/assets/image%20%2839%29%20%286%29%20%281%29%20%286%29.png)

## Questions fréquentes

###  Ignorer certains fichiers

 ****Vous pouvez éditer le fichier `wandb/settings` et régler ignore\_globs égal à une liste séparée par des virgules de [globs](https://en.wikipedia.org/wiki/Glob_%28programming%29). Vous pouvez aussi ajouter la variable d’environnement **WANDB\_IGNORE\_GLOBS**. Une utilisation fréquente est d’empêcher le patch git que nous créons automatiquement d’être téléchargé sur le cloud, i.e. **WANDB\_IGNORE\_GLOBS=\*.patch**

###  Synchroniser les fichiers avant la fin de l’essai

Si vous avez un essai long, vous pouvez avoir envie de voir des fichiers comme les checkpoints de votre modèle mis en ligne sur le cloud avant la fin de votre essai. Par défaut, nous attendons jusqu’à la fin de l’essai pour mettre la plupart des fichiers en ligne. Vous pouvez ajouter `wandb.save('*.pth')` ou simplement `wandb.save('latest.pth')`dans votre script pour envoyer ces fichiers au cloud dès qu’ils sont écrits ou mis à jour.

### Changer le dossier de sauvegarde des fichiers

Si, par défaut, vous sauvegardez des fichiers dans AWS S3 ou dans le Google Cloud Storage, vous pouvez obtenir cette erreur:`events.out.tfevents.1581193870.gpt-tpu-finetune-8jzqk-2033426287 is a cloud storage url, can't save file to wandb.`

Pour modifier le dossier d’enregistrement des fichiers d’événements TensorBoard ou d’autres fichiers que vous voudriez que nous synchronisions, sauvegardez vos fichiers dans le wandb.run.dir pour qu’ils soient synchronisés à notre cloud.

### Obtenir le nom de l’essai

Si vous souhaitez utiliser le nom de votre essai depuis votre script, vous pouvez utiliser `wandb.run.name` et vous obtiendrez le nom de votre essai – "blissful-waterfall-2" par exemple.

il faudra que vous appeliez un save de votre essai avant d’être capable d’accéder au nom affiché :

```text
run = wandb.init(...)
run.save()
print(run.name)
```

### Pousser tous les fichiers sauvegardés dans wandb

Appelez `wandb.save("*.pt")`une fois au début de votre script après votre wandb.init. Tous les fichiers qui correspondent à ce schéma seront immédiatement sauvegardés une fois qu’ils seront écrits dans wandb.run.dir.

### Retirer les fichiers locaux qui ont été synchronisés au stockage cloud

 Il y a une commande `wandb gc` que vous pouvez lancer pour retirer les fichiers locaux qui ont déjà été synchronisés au stockage cloud. Plus d’informations d’utilisation peuvent être trouvées avec \`wandb gc —help

