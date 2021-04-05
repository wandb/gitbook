---
description: >-
  Un objet comme un dictionnaire qui sauvegarde la configuration de votre
  expérience
---

# wandb.config

##  Aperçu

[![](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/colab)

Ajoutez l’objet `wandb.config` à votre script pour sauvegarder la configuration de votre entraînement : hyperparamètres, paramètres d’entrée comme le nom du dataset ou le type de modèle, et toute autre variable indépendante pour vos expériences. Cette fonction est utile pour analyser vos expériences et reproduire votre travail plus tard. Vous serez capable de regrouper vos valeurs de configuration dans l’interface web, de comparer les paramètres de vos différents essais et de voir comment ils ont affecté vos résultats. Notez bien que les mesures des résultats ou les variables dépendantes \(comme les pertes ou la précision\) devraient plutôt être sauvegardées avec `wandb.log`.

 Vous pouvez nous envoyer un dictionnaire imbriqué dans votre configuration, et nous aplatirons les noms en utilisant des points de notre côté. Nous vous recommandons d’éviter d’utiliser des points dans les noms de variables de votre configuration, et d’utiliser un tiret ou un tiret bas à la place. Une fois que vous avez créé votre dictionnaire de configuration wandb, si votre script accède à des clefs wandb.config en-dessous du root, utilisez la syntaxe `[ ]`plutôt que la syntaxe `.` .

##  Exemple simple

```python
wandb.config.epochs = 4
wandb.config.batch_size = 32
# you can also initialize your run with a config
wandb.init(config={"epochs": 4})
```

##  Initialisation efficace

Vous pouvez traiter `wandb.config` comme un dictionnaire, en mettant à jour de multiples valeurs d’un seul coup.

```python
wandb.init(config={"epochs": 4, "batch_size": 32})
# or
wandb.config.update({"epochs": 4, "batch_size": 32})
```

##  Flags Argparse

 Vous pouvez accéder au dictionnaire des arguments depuis argparse. Ça s’avère pratique pour rapidement essayer différentes valeurs d’hyperparamètres depuis la ligne de commande.

```python
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
                     help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args) # adds all of the arguments as config variables
```

## Flags Absl

Vous pouvez aussi ajouter des flags absl.

```python
flags.DEFINE_string(‘model’, None, ‘model to run’) # name, default, help
wandb.config.update(flags.FLAGS) # adds all absl flags to config
```

##  Configurations basées sur un fichier

Vous pouvez créer un fichier appelé **config-defaults.yaml,** \_\_ et il sera automatiquement chargé dans `wandb.config`

```yaml
# sample config defaults file
epochs:
  desc: Number of epochs to train over
  value: 100
batch_size:
  desc: Size of each mini-batch
  value: 32
```

Vous pouvez indiquer à wandb de charger différents fichiers de configuration avec l’argument de ligne de commande `--configs special-configs.yaml` qui chargera les paramètres depuis le fichier special-congifs.yaml.

 Voici un exemple concret : vous avez un fichier YAML avec certaines métadonnées pour cet essai, ainsi qu’un dictionnaire d’hyperparamètres dans votre script Python. Vous pouvez sauvegarder les deux dans l’objet config imbriqué :

```python
hyperparameter_defaults = dict(
    dropout = 0.5,
    batch_size = 100,
    learning_rate = 0.001,
    )

config_dictionary = dict(
    yaml=my_yaml_file,
    params=hyperparameter_defaults,
    )

wandb.init(config=config_dictionary)
```

## Dataset Identifier

Vous pouvez ajouter un identifiant unique \(comme un dièse ou un autre identifiant\) dans la configuration de vos essais pour votre dataset en le cherchant en tant que donnée entrante dans votre expérience en utilisant `wandb.config`

```yaml
wandb.config.update({'dataset':'ab131'})
```

### Mise à jour des fichiers de configuration

 Vous pouvez utiliser l’API publique pour mettre à jour votre fichier de configuration

```yaml
import wandb
api = wandb.Api()
run = api.run("username/project/run_id")
run.config["foo"] = 32
run.update()
```

### Combinaisons clef/valeur

Vous pouvez enregistrer n’importe quelle combinaison clef/valeur dans wandb.config. Elles seront différentes pour chaque type de modèles que vous entraînez. i.e.

`wandb.config.update({"my_param": 10, "learning_rate": 0.3, "model_architecture": "B"})`

##  Flags TensorFlow \(obsolète pour tensorflow v2\)

Vous pouvez ajouter des flags TensorFlow dans l’objet de configuration.

```python
wandb.init()
wandb.config.epochs = 4

flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags as config
```

