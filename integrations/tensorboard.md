# TensorBoard

W&B prend en charge le déploiement de correctifs \(patching\) sur TensorBoard pour automatiquement enregistrer toutes les métriques issues de votre script dans nos graphiques natifs.

```python
import wandb
wandb.init(sync_tensorboard=True)
```

Nous prenons en charge TensorBoard avec toutes les versions de TensorFlow. Si vous utilisez TensorBoard avec un autre framework, W&B prend en charge TensorBoard &gt; 1.14 avec PyTorch et avec TensorBoardX.

### **Métriques personnalisées**

Si vous devez enregistrer des métriques personnalisées supplémentaires qui ne sont pas enregistrées sur TensorBoard, vous pouvez appeler `wandb.log` dans votre code `wandb.log({"custom": 0.8})`

La configuration de `wandb.log` est désactivée lors de la synchronisation avec TensorBoard. Si vous voulez configurer un autre compte d’étape, vous pouvez enregistrer les métriques avec une étape de métrique comme suit :  `wandb.log({"custom": 0.8, "global_step"=global_step})`  


###  Configuration avancée

 Si vous voulez avoir plus de contrôle sur la manière dont TensorBoard est patché, vous pouvez appeler `wandb.tensorboard.patch au lieu d’ajouter sync_tensorboard=True` dans init. Vous pouvez ajouter tensorboardX=False dans cette méthode pour vous assurer que le TensorBoard Vanilla est patché, et si vous utilisez TensorBoard &gt; 1.14 avec PyTorch, vous pouvez ajouter pytorch=True pour vous assurer qu’il est patché. Ces deux options ont de petits défauts, selon les versions importées de ces bibliothèques.

Par défaut, nous synchronisons également les fichiers `tfevents` et tout fichier `.pbtxt` . Cela nous permet de lancer une instance TensorBoard pour vous. Vous verrez un [onglet TensorBoard](https://wandb.ai/site/articles/hosted-tensorboard) sur la page d’exécution. Ce comportement peut être désactivé en passant `save=False` dans `wandb.tensorboard.patch`

**Note :** si votre script instancie un FileWriter via tf.summary.create\_file\_writer, vous devez appeler soitwandb.init, soit wandb.tensorboard.patch avant de développer le FileWriter.

```python
import wandb
wandb.init()
wandb.tensorboard.patch(save=False, tensorboardX=True)
```

### **Synchronisation des essais TensorBoard antérieurs**

  Si vous avez des fichiers tfevents existants stockés localement qui ont déjà été générés en utilisant l’intégration de la bibliothèque wandb et que vous aimeriez les importer dans wandb, vous pouvez exécuter `wandb sync log_dir`, où `log_dir`, est un répertoire contenant les fichiers tfevents .

Vous pouvez aussi exécuter `wandb sync directory_with_tf_event_file`

```bash
"""This script will import a directory of tfevents files into a single W&B run.
You must install wandb from a special branch until this feature is merged
into the mainline:""" 
pip install --upgrade git+git://github.com/wandb/client.git@feature/import#egg=wandb
```

 Vous pouvez appeler ce script avec`python no_image_import.py dir_with_tf_event_file`. Cela créera un seul essai \(run\) dans wandb avec les métriques issues des fichiers d’événements dans ce répertoire. Si vous voulez exécuter ceci dans plusieurs répertoires, vous devriez exécuter ce script uniquement une fois par essai, par conséquent, un chargeur \(loader\) pourrait ressembler à ceci :

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

Nous aspirons à améliorer les outils de suivi d’expérience pour tout le monde. Lorsque nos cofondateurs ont commencé à travailler sur W&B, ils ont voulu construire un outil pour les utilisateurs frustrés de TensorBoard qui travaillaient chez OpenAI. Voici quelques points sur lesquels nous avons concentré nos efforts d’amélioration :

1. **Reproduire les modèles** : Weights & Biases est efficace pour expérimenter, explorer et reproduire les modèles ultérieurement. Nous enregistrons non seulement les métriques, mais aussi les hyperparamètres et la version du code, et nous pouvons sauvegarder les checkpoints de votre modèle pour vous pour que votre projet soit reproductible.
2.  **Organisation automatique** : si vous passez un projet à un collaborateur ou que vous partez en vacances, W&B facilite la visualisation de tous les modèles que vous avez déjà essayés, ce qui vous évite de passer des heures à réexécuter d’anciennes expériences.
3.  **Intégration rapide et flexible** : ajoutez W&B à votre projet en 5 minutes. Installez notre package Python gratuitement en open-source et ajoutez quelques lignes à votre code, et à chaque fois que vous essaierez votre modèle, vous aurez d’excellents enregistrements de données et de métriques.
4. **Tableau de bord centralisé permanent** : quel que soit l’emplacement où vous souhaitez entraîner vos modèles, que ce soit sur votre ordinateur local, dans la grappe de serveurs \(cluster\) de votre Lab, ou pour des instances ponctuelles dans le cloud, nous vous fournissons le même tableau de bord centralisé. Vous n’avez pas besoin de passer votre temps à copier et à organiser des fichiers TensorBoard depuis différentes machines.
5.  **Tableau puissant** : recherchez, filtrez, organisez et regroupez vos résultats depuis différents modèles. Il facilite la visualisation de milliers de versions de modèle et la recherche de ceux qui offrent les meilleures performances dans différentes tâches. TensorBoard n’est pas conçu pour bien fonctionner sur de grands projets.
6. **Des outils dédiés à la collaboration** : utilisez W&B pour organiser des projets complexes d’apprentissage automatique. Il est facile de partager un lien vers W&B, et vous pouvez utiliser la fonction d’équipe privée pour que tout le monde puisse envoyer des résultats sur un projet en commun. Nous soutenons aussi la collaboration via des rapports – ajoutez des visuels interactifs et décrivez votre travail dans un Markdown. C’est une excellente manière de maintenir un journal de bord, partager vos résultats avec votre superviseur, ou de présenter vos résultats à votre Lab.

  

Commencez en créant un [compte personnel gratuit →](http://app.wandb.ai/)

