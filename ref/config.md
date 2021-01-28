# Config

## wandb.sdk.wandb\_config

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_config.py#L3)

Config est un objet comme un dictionnaire utile pour garder une trace des entrées \(inputs\) de votre script, comme les hyperparamètres. Nous suggérons que vous le paramétriez une fois au début de votre tâche, lorsque vous initialisez votre essai, comme ceci : `wandb.init(config={"key": "value"})`  . Par exemple, si vous entraînez un modèle d’apprentissage automatique, vous pouvez garder une trace du learning\_rate \(taux d’apprentissage\) ou de la batch\_size \(taille de lots\) dans config.

###  Objets Config

```python
class Config(object)
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_config.py#L32)

Utilisez l’objet config pour sauvegarder les hyperparamètres de votre essai. Lorsque vous appelez wandb.init\(\) pour commencer un nouvel essai enregistré, un objet run est sauvegardé. Nous recommandons de sauvegarder l’objet config avec celui run au même moment, comme suit :`wandb.init(config=my_config_dict)`.

Vous pouvez créer un fichier appelé config-defaults.yaml, et wandb chargera automatiquement votre config dans wandb.config. Alternativement, vous pouvez utiliser un fichier YAML avec un nom personnalisé et faire passer ce nom de fichier : `wandb.init(config="my_config_file.yaml")` Voir [https://docs.wandb.com/library/config\#file-based-configs](https://docs.wandb.com/library/config#file-based-configs).

**Exemples :**

Utilisation basique

```text
wandb.config.epochs = 4
wandb.init()
for x in range(wandb.config.epochs):
# train
```

Utiliser wandb.init pour paramétrer config

```text
- `wandb.init(config={"epochs"` - 4, "batch_size": 32})
for x in range(wandb.config.epochs):
# train
```

Configs imbriquées

```text
wandb.config['train']['epochs] = 4
wandb.init()
for x in range(wandb.config['train']['epochs']):
# train
```

Utiliser flags absl

```text
flags.DEFINE_string(‘model’, None, ‘model to run’) # name, default, help
wandb.config.update(flags.FLAGS) # adds all absl flags to config
```

Flags argparse

```text
wandb.init()
wandb.config.epochs = 4

parser = argparse.ArgumentParser()
parser.add_argument('-b', '--batch-size', type=int, default=8, metavar='N',
help='input batch size for training (default: 8)')
args = parser.parse_args()
wandb.config.update(args)
```

Utiliser flags TensorFlow \(obsolète dans tensorflow v2\)

```text
flags = tf.app.flags
flags.DEFINE_string('data_dir', '/tmp/data')
flags.DEFINE_integer('batch_size', 128, 'Batch size.')
wandb.config.update(flags.FLAGS)  # adds all of the tensorflow flags to config
```

**persist**

```python
 | persist()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_config.py#L162)

Appelle le callback s’il est paramétré

