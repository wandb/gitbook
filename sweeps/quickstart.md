# Sweeps Quickstart

Commencez à partir de n’importe quel modèle d’apprentissage automatique et mettez en place un balayage d’hyperparamètres en quelques minutes. Vous voulez voir un exemple en action ? Voici un [exemple de code](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion) et un [exemple de tableau de bord](https://app.wandb.ai/carey/pytorch-cnn-fashion/sweeps/v8dil26q).

![](../.gitbook/assets/image%20%2847%29%20%282%29%20%283%29%20%284%29%20%283%29%20%283%29.png)

{% hint style="info" %}
\(i\) Vous avez déjà un projet Weights & Biases ? [Passez directement à notre tutoriel suivant de Balayages →](https://docs.wandb.ai/sweeps/existing-project)
{% endhint %}

## 1.  Ajouter Wandb

### Paramétrer votre compte

1. Commencez avec un compte W&B. [Créez-en un dès maintenant →](http://app.wandb.ai/)
2. Rendez-vous sur votre terminal, dans votre dossier de projet, et installer notre librairie : `pip install wandb`
3. À l’intérieur de votre dossier de projet, connectez-vous à W&B : `wandb login`

### Paramétrer votre script d’entraînement Python

1. Importez notre librairie `wandb`  
2. Assurez-vous que vos hyperparamètres puissent bien être paramétrés par le balayage. Définissez-les dans un dictionnaire en haut de votre script et passez-les dans wandb.init.
3. Enregistrez des mesures pour les voir en direct sur votre tableau de bord.

```python
import wandb

# Set up your default hyperparameters before wandb.init
# so they get properly set in the sweep
hyperparameter_defaults = dict(
    dropout = 0.5,
    channels_one = 16,
    channels_two = 32,
    batch_size = 100,
    learning_rate = 0.001,
    epochs = 2,
    )

# Pass your defaults to wandb.init
wandb.init(config=hyperparameter_defaults)
config = wandb.config

# Your model here ...

# Log metrics inside your training loop
metrics = {'accuracy': accuracy, 'loss': loss}
wandb.log(metrics)
```

 [Voir un exemple de code intégral →](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion)

## 2.  Config de balayage

Mettez en place un **fichier YAML** pour spécifier votre script d’entraînement, vos plages de paramètres, votre stratégie de recherche, et vos critères d’arrêt. W&B passera ces paramètres et leurs valeurs comme des arguments de ligne de commande à votre script d’entraînement, et nous les analyserons automatiquement avec l’objet config que vous avez mis en place à [l’étape 1](https://docs.wandb.ai/sweeps/quickstart#set-up-your-python-training-script).

Voici quelques ressources de config :

1. [Exemples de fichiers YAML ](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion): un script d’exemple et plusieurs fichiers YAML différents
2. [Configuration](https://docs.wandb.ai/sweeps/configuration) : specs complètes pour mettre en place votre config de balayage
3. [Jupyter Notebook](python-api.md): mettez en place vos configs de balayage avec un dictionnaire Python plutôt qu’un fichier YAML
4. [Générer la config depuis l’IU ](https://docs.wandb.ai/sweeps/existing-project): prenez un projet W&B existant et générez un fichier de config
5. [Ajouter des essais antérieurs ](https://docs.wandb.com/sweeps/existing-project#seed-a-new-sweep-with-existing-runs): prenez d’anciens essais et ajoutez-les à un nouveau balayage

 Voici un exemple de fichier YAML de config de balayage appelé **sweep.yaml** :

```text
program: train.py
method: bayes
metric:
  name: val_loss
  goal: minimize
parameters:
  learning_rate:
    min: 0.001
    max: 0.1
  optimizer:
    values: ["adam", "sgd"]
```

{% hint style="warning" %}
Si vous spécifiez une mesure à optimiser, assurez-vous que vous l’enregistriez. Dans cet exemple, j’ai **val\_loss** dans mon fichier de config, il faut donc que j’enregistre une mesure avec ce nom exact dans mon script :

`wandb.log({"val_loss": validation_loss})`
{% endhint %}

Sous le capot, cette configuration utilisera la méthode d’optimisation Bayes pour choisir des sets de valeurs d’hyperparamètres avec lesquelles appeler votre programme. Elle lancera des expériences avec la syntaxe suivante :

```text
python train.py --learning_rate=0.005 --optimizer=adam
python train.py --learning_rate=0.03 --optimizer=sgd
```

{% hint style="info" %}
Si vous utilisez argparse dans votre script, nous vous recommandons d’utiliser des tirets du bas dans vos noms de variables plutôt que des tirets du haut.
{% endhint %}

## 3. Initialiser un balayage

Notre serveur central s’occupe de la coordination entre tous les agents qui exécutent le balayage. Paramétrez un fichier de config de balayage et exécutez cette commande pour commencer :

```text
wandb sweep sweep.yaml
```

Cette commande imprimera une **ID de balayage** \(sweep ID\), qui inclue le nom d’entité et le nom de projet. Copiez-la pour l’utiliser à l’étape suivante !

## 4. Lancer un ou plusieurs agents

Sur chaque machine sur laquelle vous voulez exécuter ce balayage, lancez un agent avec l’ID de balayage. Vous voudrez utiliser la même ID de balayage pour tous les agents qui effectuent le même balayage.

Dans une shell sur votre propre machine, exécutez la commande wandb agent qui demandera au serveur quelles commandes exécuter :

```text
wandb agent your-sweep-id
```

Vous pouvez exécuter un agent wandb sur plusieurs machines ou dans plusieurs processus sur la même machine, et chaque agent interrogera le serveur central de balayage de W&B pour obtenir le prochain set d’hyperparamètres à exécuter.

### 5. Visualiser les résultats

Ouvrez votre projet pour voir les résultats en direct dans le tableau de bord de bal

 [Exemple de tableau de bord →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

![](../.gitbook/assets/image%20%2888%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29%20%283%29%20%283%29.png)

