# TensorBoard

W&B prend en charge le patching TensorBoard pour automatiquement enregistrer toutes les mesures issues de votre script dans nos graphiques de manière native.

```python
import wandb
wandb.init(sync_tensorboard=True)
```

Nous prenons en charge TensorBoard avec toutes les versions de TensorFlow. Si vous utilisez TensorBoard avec un autre framework, W&B prend en charge TensorBoard &gt; 1.14 avec PyTorch, ainsi qu’avec TensorBoardX.

### Mesures personnalisées

Si vous avez besoin d’enregistrer des mesures personnalisées supplémentaires qui ne sont pas enregistrées dans TensorBoard, vous pouvez appeler `wandb.log` dans votre code, à la même étape d’argument que celle utilisée par TensorBoard : i.e.`wandb.log({"custom": 0.8}, step=global_step)`

###  Configuration avancée

 Si vous voulez avoir plus de contrôle sur la manière dont TensorBoard est patché, vous pouvez appeler `wandb.tensorboard.patch plutôt que de passer sync_tensorboard=True` dans init. Vous pouvez passer `tensorboardX=False` dans cette méthode pour vous assurer que le TensorBoard vanilla est patché, et si vous utilisez TensorBoard &gt; 1.14 avec PyTorch, vous pouvez passer `pytorch=True` pour vous assurer qu’il est patché. Ces deux options ont de petits défauts, dépendant de quelles versions de ces librairies ont été importées.

 Par défaut, nous synchronisons aussi les fichiers `tfevents` et tout fichier `.pbtxt` . Cela nous permet de lancer une instance TensorBoard pour vous. Vous verrez un onglet [TensorBoard sur la page](https://wandb.ai/site/articles/hosted-tensorboard) d’essai. Ce comportement peut être désactivé en passant `save=False` dans `wandb.tensorboard.patch`

```python
import wandb
wandb.init()
wandb.tensorboard.patch(save=False, tensorboardX=True)
```

### Synchroniser des essais TensorBoard antérieurs

 Si vous avez des fichiers `tfevents` existants stockés localement qui ont déjà été générés en utilisant l’intégration de librairie wandb et que vous aimeriez les importer dans wandb, vous pouvez exécuter wandb sync log\_dir , où log\_dir est un répertoire contenant les fichiers `tfevents` .

Vous pouvez aussi exécuter `wandb sync directory_with_tf_event_file`

```bash
"""This script will import a directory of tfevents files into a single W&B run.
You must install wandb from a special branch until this feature is merged
into the mainline:""" 
pip install --upgrade git+git://github.com/wandb/client.git@feature/import#egg=wandb
```

Vous pouvez appeler ce script avec **``**`python no_image_import.py dir_with_tf_event_file`. Cela créera un simple essai \(run\) dans wandb avec les mesures issues des fichiers d’événements dans ce répertoire. Si vous voulez exécuter ceci dans plusieurs répertoires, vous ne devriez exécuter ce script qu’une fois par essai, de sorte qu’un loader ressemble à ceci :

```python
import glob
for run_dir in glob.glob("logdir-*"):
  subprocess.Popen(["python", "no_image_import.py", run_dir],
                   stderr=subprocess.PIPE, stdout=subprocess.PIPE)
```

```python
import glob
import os
import wandb
import sys
import time
import tensorflow as tf
from wandb.tensorboard.watcher import Consumer, Event
from six.moves import queue

if len(sys.argv) == 1:
    raise ValueError("Must pass a directory as the first argument")

paths = glob.glob(sys.argv[1]+"/*/.tfevents.*", recursive=True)
root = os.path.dirname(os.path.commonprefix(paths)).strip("/")
namespaces = {path: path.replace(root, "")\
              .replace(path.split("/")[-1], "").strip("/")
              for path in paths}
finished = {namespace: False for path, namespace in namespaces.items()}
readers = [(namespaces[path], tf.train.summary_iterator(path)) for path in paths] 
if len(readers) == 0: 
    raise ValueError("Couldn't find any event files in this directory")

directory = os.path.abspath(sys.argv[1])
print("Loading directory %s" % directory)
wandb.init(project="test-detection")

Q = queue.PriorityQueue()
print("Parsing %i event files" % len(readers))
con = Consumer(Q, delay=5)
con.start()
total_events = 0

while True:

    # Consume 500 events at a time from all readers and push them to the queue
    for namespace, reader in readers:
        if not finished[namespace]:
            for i in range(500):
                try:
                    event = next(reader)
                    kind = event.value.WhichOneof("value")
                    if kind != "image":
                        Q.put(Event(event, namespace=namespace))
                        total_events += 1
                except StopIteration:
                    finished[namespace] = True
                    print("Finished parsing %s event file" % namespace)
                    break
    if all(finished.values()):
        break

print("Persisting %i events..." % total_events)
con.shutdown(); print("Import complete")
```

### Google Colab et TensorBoard

To run commands from the command line in Colab, you must run `!wandb sync directoryname` . Currently TensorBoard syncing does not work in a notebook environment for Tensorflow 2.1+. You can have your Colab use an earlier version of TensorBoard, or run a script from the command line with `!python your_script.py` .

Pour exécuter des commandes sur la ligne de commande dans Colab, vous devez exécuter ! wandb sync directoryname \(nom de répertoire\). Pour l’instant, la synchronisation TensorBoard ne fonctionne pas dans un environnement notebook pour Tensorflow 2.1+. Vous pouvez faire en sorte que votre Colab utilise une version antérieure de TensorBoard, ou exécuter un script depuis la ligne de commande avec !python your\_script.py .

##  En quoi W&B est-il différent de TensorBoard ?

Nous avons voulu améliorer les outils de traçage d’expérience pour tout le monde. Lorsque les cofondateurs ont commencé à travailler sur W&B, ils ont voulu construire un outil pour les utilisateurs frustrés de TensorBoard qui travaillaient à OpenAI. Voici quelques points sur lesquels nous avons concentré nos efforts d’amélioration :

1. **Reproduire les modèles** : Weights & Biases est efficace pour expérimenter, explorer, et reproduire les modèles plus tard. Nous enregistrons non seulement les mesures, mais aussi les hyperparamètres et la version du code, et nous pouvons sauvegarder les checkpoints de votre modèle pour vous pour que votre projet soit reproductible.
2.  **Organisation automatique** : Si vous passez un projet à un collaborateur ou que vous partez en vacances, W&B rend facile la visualisation de tous les modèles que vous avez déjà essayés, pour que vous ne passiez pas des heures à remodéliser d’anciennes expériences.
3.  **Intégration rapide et flexible** : Ajoutez W&B à votre projet en 5 minutes. Installez notre package Python gratuit et open-source et ajoutez quelques lignes à votre code, et à chaque fois que vous essaierez votre modèle, vous aurez de magnifiques enregistrements de données et de mesures.
4. **Persistent, centralized dashboard**: Anywhere you train your models, whether on your local machine, your lab cluster, or spot instances in the cloud, we give you the same centralized dashboard. You don't need to spend your time copying and organizing TensorBoard files from different machines.
5.  **Tableau puissant** : Recherchez, filtrez, organisez, et regroupez vos résultats depuis différents modèles. Il est facile de visualiser des milliers de versions de modèle et de trouver ceux qui offrent les meilleures performances dans différentes tâches. TensorBoard n’est pas construit pour bien fonctionner sur de grands projets.
6. **Des outils pour la collaboration** : Utilisez W&B pour organiser des projets complexes d’apprentissage automatique. Il est facile de partager un lien vers W&B, et vous pouvez utiliser des équipes privées pour que tout le monde envoie des résultats sur un projet en commun. Nous soutenons aussi la collaboration par les rapports – ajoutez des visuels interactifs et décrivez votre travail dans un Markdown. C’est une manière excellente de garder un journal de travail, de partager vos découvertes avec votre superviseur, ou de présenter vos découvertes à votre laboratoire. 

Commencez en créant un [compte personnel gratuit →](http://app.wandb.ai/)

