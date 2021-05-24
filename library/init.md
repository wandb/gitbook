---
description: Appelez wandb.init() au début de votre script pour commencer un nouvel essai
---

# wandb.init\(\)

 Appelez `wandb.init()` une fois au début de votre script pour initialiser une nouvelle tâche. Cela crée un nouvel essai dans W&B et le processus de synchronisation des données se met en marche en arrière-plan.

*  **Sur site** : si vous avez besoin d’un cloud privé ou d’une instance locale de W&B, consultez nos offres[d’auto-hébergement](https://docs.wandb.ai/v/fr/self-hosted).
* **Environnements automatisés** : la plupart de ces paramètres peuvent également être contrôlés via des [variables d’environnement](https://docs.wandb.ai/v/fr/library/environment-variables). C’est souvent utile lorsque vous effectuez des tâches sur un groupe.

###  Documents de référence

Consultez les documents de référence pour les arguments.

{% page-ref page="../ref/init.md" %}

##  Questions fréquentes

###  Comment puis-je lancer plusieurs essais à partir d’un seul script ?

 Si vous essayez de lancer plusieurs essais à partir d’un seul script, ajoutez deux éléments à votre code :

1.  run=wandb.init\(**reinit=True**\) : utilisez ce paramétrage pour permettre la réinitialisation des essais
2. **run.finish\(\)** : utilisez ceci à la fin de votre essai pour terminer l’enregistrement de cet essai

```python
import wandb
for x in range(10):
    run = wandb.init(project="runs-from-for-loop", reinit=True)
    for y in range (100):
        wandb.log({"metric": x+y})
    run.finish()
```

Vous pouvez également utiliser un gestionnaire de contexte python qui terminera automatiquement l’enregistrement :

```python
import wandb
for x in range(10):
    run = wandb.init(reinit=True)
    with run:
        for y in range(100):
            run.log({"metric": x+y})
```

###  LaunchError: Permission denied

Si vous recevez un message d’erreur **LaunchError: Launch exception: Permission denied,** c’est que vous n’avez pas les autorisations pour vous connecter au projet sur lequel vous voulez envoyer vos essais. Cela peut être dû à plusieurs raisons.

1. Vous n’êtes pas connecté sur cette machine. Utilisez `wandb login` sur la ligne de commande.
2.  Vous avez paramétré une entité qui n’existe pas. « Entity » doit être votre nom d’utilisateur ou le nom d’une équipe qui existe. Si vous avez besoin de créer une équipe, rendez-vous sur notre [Page d’Inscription](https://app.wandb.ai/billing).
3.  Vous n’avez pas d’autorisation sur ce projet. Demandez au créateur du projet de régler la confidentialité sur **Ouvert \(Open\)** pour que vous puissiez ajouter vos essais à ce projet.

###  **InitStartError: Error communicating with wandb process**[\[NFR1\]](applewebdata://67CC1A5F-0653-4B12-8AD3-A75C70D570B6#_msocom_1) 

Le message d’erreur **InitStartError: Error communicating with wandb process** indique que la bibliothèque rencontre des difficultés pour lancer le processus de synchronisation des données au serveur. 

Les solutions de contournement suivantes peuvent résoudre ce problème dans certains environnements :

### **Le multitraitement n’est pas encore pris en charge**

Si votre programme d’entraînement utilise un système de multitraitement, vous devez structurer votre programme de façon à éviter d’effectuer des appels de méthode wandb à partir des processus où vous ne souhaitez pas exécuter wandb.init\(\). Il existe plusieurs approches pour gérer les entraînements à multitraitement :

1. Appeler `wandb.init()` en spécifiant un groupe courant pour tous vos processus. Chaque processus aura son propre essai wandb et l’interface utilisateur regroupera tous les processus d’entraînement.
2. Appeler `wandb.init()` à partir d’un seul processus et transmettre les données à enregistrer à travers des files d’attente de multitraitement. 

## **Obtenir un nom lisible pour mon essai**

Obtenez un nom lisible et agréable pour votre essai.

```python
import wandb
wandb.init()
run_name = wandb.run.name
```

## **Paramétrer le nom de l’essai sur son ID généré automatiquement**

 Si vous préférez remplacer le nom de votre essai \(comme chouette-blanche-10\) par l’ID de l’essai \(comme qvlp96vk\), vous pouvez utiliser ce snippet :

```python
import wandb
wandb.init()
wandb.run.name = wandb.run.id
wandb.run.save()
```

###  **Sauvegarder le git commit**

 ****Lorsque vous appelez wandb.init\(\) dans votre script, nous recherchons automatiquement les informations git pour sauvegarder le lien SHA de votre dernier commit dans votre répertoire \(repo\). L’information git devrait s’afficher sur votre [page d’exécution \(run](https://docs.wandb.ai/v/fr/app/pages/run-page) page\). Si vous ne la voyez pas apparaître, assurez-vous que le script dans lequel vous appelez wandb.init\(\) est placé dans un dossier qui contient les informations git.

 Le git commit et la commande utilisée pour l’essai de votre expérience sont visibles pour vous, mais cachés aux utilisateurs externes. Ce qui gardera ces détails privés, si vous avez un projet public.

###  **Sauvegarder des enregistrements hors-ligne**

Par défaut, wandb.init\(\) débute un processus qui synchronise les métriques en temps réel à notre application hébergée sur cloud. Si votre appareil est hors-ligne ou n’a pas accès à internet, voici comment exécuter wandb en utilisant le mode hors-ligne et en synchronisant plus tard.

 Ajoutez deux variables d’environnement :

1. **WANDB\_API\_KEY** : paramétrez ceci à la clé API de votre compte, sur votre [page de paramètres](https://app.wandb.ai/settings)​
2. **WANDB\_MODE** : test à blanc \(dryrun\)

Voici un exemple de ce que ça donnerait dans votre script :

```python
import wandb
import os

os.environ["WANDB_API_KEY"] = YOUR_KEY_HERE
os.environ["WANDB_MODE"] = "dryrun"

config = {
  "dataset": "CIFAR10",
  "machine": "offline cluster",
  "model": "CNN",
  "learning_rate": 0.01,
  "batch_size": 128,
}

wandb.init(project="offline-demo")

for i in range(100):
  wandb.log({"accuracy": i})
```

Voici un exemple de résultats sur terminal :

![](../.gitbook/assets/image%20%2881%29.png)

Et une fois que j’ai de nouveau accès à internet, j’exécute une commande sync pour envoyer ce dossier au cloud.

`wandb sync wandb/dryrun-folder-name`

![](../.gitbook/assets/image%20%2836%29.png)

