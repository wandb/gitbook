# Sweeps Quickstart

 Commencez à partir de n’importe quel modèle d’apprentissage automatique et configurez un balayage d’hyperparamètres en quelques minutes. Vous voulez voir un exemple opérationnel ? Voici un [exemple de code](https://github.com/wandb/examples/tree/master/examples/pytorch/pytorch-cnn-fashion) et un [exemple de tableau de bord](https://app.wandb.ai/carey/pytorch-cnn-fashion/sweeps/v8dil26q).

![](../.gitbook/assets/image%20%2847%29%20%282%29%20%283%29%20%284%29%20%283%29%20%283%29.png)

{% hint style="info" %}
\(i\) Vous avez déjà un projet sur Weights & Biases ?[ Passez directement à notre tutoriel de balayages suivant→](https://docs.wandb.ai/v/fr/sweeps/existing-project)​
{% endhint %}

## 1.  Ajouter Wandb

### Paramétrer votre compte

1. Commencez par un compte W&B. [Créez-en un maintenant →](http://app.wandb.ai/)​
2. Rendez-vous sur votre terminal, dans votre dossier de projet et installer notre bibliothèque :`pip install wandb`
3. À l’intérieur de votre dossier de projet, connectez-vous à W&B : `wandb login`

### Paramétrer votre script d’entraînement Python

1. Importez notre bibliothèque `wandb`
2. Assurez-vous que vos hyperparamètres puissent être correctement paramétrés par le balayage. Définissez-les dans un dictionnaire au début de votre script et ajoutez-les à wandb.init.
3. Enregistrez des métriques pour les voir en temps réel sur votre tableau de bord.

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

## 2.  **Configuration de balayage**

 Mettez en place un **fichier YAML** pour spécifier votre script d’entraînement, vos plages de paramètres, votre stratégie de recherche et vos critères d’arrêt. W&B ajoutera ces paramètres et leurs valeurs en tant qu’arguments de ligne de commande à votre script d’entraînement, et nous les analyserons automatiquement avec l’objet config que vous avez mis en place à [l’étape 1](https://docs.wandb.ai/sweeps/quickstart#set-up-your-python-training-script).

Voici quelques ressources de configuration :

1. [Exemples de fichiers YAML ](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion): un script d’exemple et différents fichiers YAML
2. [Configuration](https://docs.wandb.ai/sweeps/configuration) : spécifications complètes pour mettre en place votre configuration de balayage
3. [Jupyter Notebook](python-api.md): met en place vos configurations de balayage avec un dictionnaire Python au lieu d’un fichier YAML
4. [​Générer la configuration depuis l’interface utilisateur](https://docs.wandb.ai/v/fr/sweeps/existing-project) : génère un fichier de configuration à partir d’un projet W&B existant
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
Si vous spécifiez une métrique à optimiser, assurez-vous de l’enregistrer. Dans cet exemple, j’ai **val\_loss**dans mon fichier de configuration, il faut donc que j’enregistre une métrique avec ce nom exact dans mon script :  
`wandb.log({"val_loss": validation_loss})`
{% endhint %}

De manière sous-jacente, cette configuration utilisera la méthode d’optimisation bayésienne pour choisir des sets de valeurs d’hyperparamètres auxquelles votre programme sera appelé. Elle lancera des expériences avec la syntaxe suivante :

```text
python train.py --learning_rate=0.005 --optimizer=adam
python train.py --learning_rate=0.03 --optimizer=sgd
```

{% hint style="info" %}
Si vous utilisez argparse dans votre script, nous vous recommandons d’utiliser des tirets du bas dans les noms de vos variables au lieu de traits d’union.
{% endhint %}

## 3. Initialiser un balayage

Notre serveur central coordonne tous les agents qui exécutent le balayage. Paramétrez un fichier de configuration de balayage et exécutez cette commande pour commencer :

```text
wandb sweep sweep.yaml
```

Cette commande affichera une **ID de balayage \(sweep ID\),** qui inclue le nom d’entité et le nom de projet. Copiez-la pour l’utiliser à l’étape suivante !

## 4. Lancer un ou plusieurs agents

Sur chaque ordinateur sur lequel vous voulez exécuter ce balayage, lancez un agent avec l’ID de balayage. Vous devez utiliser la même ID de balayage pour tous les agents qui effectuent le même balayage.

Dans une interface système \(shell\) sur votre propre ordinateur, exécutez la commande wandb agent qui demandera au serveur quelles commandes exécuter :

```text
wandb agent your-sweep-id
```

Vous pouvez exécuter un agent wandb sur plusieurs ordinateurs ou dans plusieurs processus sur le même ordinateur, et chaque agent demandera au serveur central de balayage de W&B quel est le prochain set d’hyperparamètres à exécuter.

### 5. Visualiser les résultats

Ouvrez votre projet pour voir les résultats en direct dans le tableau de bord de balayage

 [Exemple de tableau de bord →](https://app.wandb.ai/carey/pytorch-cnn-fashion)

![](../.gitbook/assets/image%20%2888%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29%20%283%29%20%284%29.png)

**6. Arrêter un agent**

À partir du terminal, vous pouvez appuyer sur Ctrl+C pour arrêter l’essai sur lequel l’agent de balayage s’exécute. Si vous voulez supprimer l’agent, vous devez appuyer à nouveau sur Ctrl+C après l’arrêt de l’essai.

