---
description: >-
  Identifiez-vous, restaurez l’état de votre code, synchronisez vos dossiers
  locaux sur nos serveurs, et exécutez des balayages d’hyperparamètres avec
  notre interface de ligne de commande
---

# Command Line Interface

 Après avoir exécuté `pip install wandb`, vous devriez avoir une nouvelle commande disponible, **wandb**.

Les sous-commandes suivantes sont disponibles :

| Sous-commande | Description |
| :--- | :--- |
| docs | Ouvre la documentation dans un navigateur |
| init | Configure un répertoire avec W&B |
| login | Ouvre une session sur W&B |
| offline | Sauvegarde les données de l’essai localement, sans synchronisation cloud \( `off` obsolète\) |
| online | S’assure que W&B est activé dans ce dossier \( `on` obsolète\) |
| disabled | Désactive tous les appels API, utile pour faire des tests |
| enabled | Même chose que online, reprend un enregistrement normal W&B, une fois que vous avez fini vos tests. |
| docker | Exécute une image docker, monte le répertoire de travail en cours \(cwd\), et s’assure que wandb est installée |
| docker-run | Ajoute des variables d’environnement W&B à une commande d’exécution de Docker |
| projects | Liste les projets |
| pull | Extrait des fichiers d’un essai depuis W&B |
| restore | Restaure le code et l’état config pour un essai |
| run | Lance un programme non-python. Pour python, utiliser wandb.init\(\) |
| runs | Liste les essais \(runs\) dans un projet |
| sync | Synchronise un répertoire local contenant des événements TensorFlow \(TFEvents\) ou des fichiers d’essais précédents |
| status | Liste les statuts du répertoire actuel |
| sweep | Crée un nouveau balayage avec une définition YAML à donner |
| agent | Démarre un agent pour exécuter des programmes dans le balayage |

## Restaurer l’état de votre code

Utilisez `restore` pour retourner à l’état de votre code lorsque vous avez fait un essai \(run\) particulier.

### Exemple

```python
# creates a branch and restores the code to the state it was in when run $RUN_ID was executed
wandb restore $RUN_ID
```

**Comment capturons-nous l’état du code ?**

 Lorsque `wandb.init` est appelé depuis votre script, un lien est sauvegardé dans le dernier git commit si votre code est dans un répertoire git. Un patch diff est aussi créé, au cas-où il y aurait des changements non affectés, ou des changements qui seraient non synchronisés à vos systèmes à distance.

