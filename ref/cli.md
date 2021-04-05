# Command Line Reference

## wandb

 **Utilisation**

`wandb [OPTIONS] COMMAND [ARGS]...`

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --version | Montre la version et quitte. |
| --help | Montre ce message et quitte. |

**Commandes**

| **Commandes** | **Description** |
| :--- | :--- |
| agent | Exécute l’agent W&B |
| artifact | Commandes pour interagir avec les artefacts |
| controller | Exécute le contrôleur de balayage local W&B |
| disabled | Désactive W&B. |
| docker | docker vous permet d’exécuter votre code dans une image docker, vous permettant… |
| docker-run | Wrapper simple pour `docker run qui met en place l’environnement W&B`... |
| enabled | Active W&B. |
| init | Configure un répertoire avec Weights & Biases |
| local | Lance un container W&B local \(Expérimental\) |
| login | Se connecte à Weights & Biases |
| offline | Désactive la synchro W&B |
| online | Active la synchro W&B |
| pull | Tire des fichiers depuis Weights & Biases |
| restore | Restaure l’état de code, de config et de docker pour un run |
| status | Montre les paramètres de configuration |
| sweep | Crée un balayage |
| sync | Met en ligne un répertoire d’entraînement hors-ligne sur W&B. |

## wandb agent

**Utilisation**

`wandb agent [OPTIONS] SWEEP_ID`

**Sommaire**

Exécute l’agent W&B

**Options**

| **Options** | **Description** |
| :--- | :--- |
| -p, --project | Le projet du balayage |
| -e, --entity | Le scope d’entité pour le projet. |
| --count | Le nombre max de runs pour cet agent. |
| --help | Montre ce message et quitte. |

## wandb artifact

**Utilisation**

`wandb artifact [OPTIONS] COMMAND [ARGS]...`

**Sommaire**

Commandes pour interagir avec les artefacts

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --help | Montre ce message et quitte. |

### wandb artifact get

**Utilisation**

`wandb artifact get [OPTIONS] PATH`

**Sommaire**

 Télécharge un artefact depuis wandb

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --root | Le répertoire dans lequel vous voulez télécharger l’artefact |
| --type | Le type d’artefact que vous téléchargez |
| --help | Montre ce message et quitte. |

### wandb artifact ls

**Utilisation**

`wandb artifact ls [OPTIONS] PATH`

**Sommaire**

Liste tous les artefacts dans un projet wandb

**Options**

| **Options** | **Description** |
| :--- | :--- |
| -t, --type | Le type d’artefacts à lister |
| --help | Montre ce message et quitte. |

### wandb artifact put

**Utilisation**

`wandb artifact put [OPTIONS] PATH`

**Sommaire**

 Télécharge un artefact vers wandb

**Options**

| **Options** | **Description** |
| :--- | :--- |
| -n, --name | Le nom de l’artefact à push |
| -d, --description | Une description de cet artefact |
| -t, --type | Le type de l’artefact |
| -a, --alias | Un alias qui s’applique à cet artefact |
| --help | Montre ce message et quitte. |

## wandb controller

**Usage**

`wandb controller [OPTIONS] SWEEP_ID`

**Utilisation**

Exécute le contrôleur local de balayage W&B.

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --verbose | Affiche l’output verbose |
| --help | Montre ce message et quitte |

## wandb disabled

**Utilisation**

`wandb disabled [OPTIONS]`

**Sommaire**

Désactive W&B.

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --help | Montre ce message et quitte |

## wandb docker

 **Utilisation**

`wandb docker [OPTIONS] [DOCKER_RUN_ARGS]... [DOCKER_IMAGE]`

 **Sommaire**

W&B docker vous permet d’exécuter votre code dans une image docker qui s’assure que wandb est configuré. Cela ajoute les variables d’environnement WANDB\_DOCKER etWANDB\_API\_KEY à votre containeur et monte le répertoire actuel dans /app par défaut. Vous pouvez passer des args supplémentaires qui seront ajoutés au `docker run` avant que le nom de l’image soit déclaré, nous choisirons une image par défaut pour vous si aucune n’est passée :

wandb docker -v /mnt/dataset:/app/data wandb docker gcr.io/kubeflow- images-public/tensorflow-1.12.0-notebook-cpu:v0.4.0 --jupyter wandb docker wandb/deepo:keras-gpu --no-tty --cmd "python train.py --epochs=5"

Par défaut, nous écrasons le point d’entrée \(entrypoint\) pour verifier l’existence de wandb et l’installer s’il n’est pas présent. Si vous passez le flag --jupyter nous nous assurerons que jupyter est installé et nous lancerons jupyter lab sur le port 8888. Si nous détectons nvidia-docker sur votre système, nous utiliserons le runtime nvidia. Si vous voulez simplement que wandb paramètre la variable d’environnement sur une commande docker run existante, voir la commande wandb docker-run.

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --nvidia | / --no-nvidia Utilise le runtime nvidia, par défaut nvidia si |
| nvidia-docker | est présent |
| --digest | Output le digest d’image et quitte |
| --jupyter | / --no-jupyter Exécute jupyter lab dans le containeur |
| --dir | Dans quel répertoire monter le code dans le containeur |
| --no-dir | Ne monte pas le répertoire courant |
| --shell | Le shell avec lequel commencer le containeur |
| --port | Le port d’hôte sur lequel lier jupyter |
| --cmd | La commande à exécuter dans le container |
| --no-tty | Exécuter la commande sans un tty |
| --help | Montre ce message et quitte. |

## wandb enabled

**Utilisation**

`wandb enabled [OPTIONS]`

 **Sommaire**

Active W&B.

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --help | Montre ce message et quitte. |

## wandb init

 **Utilisation**

`wandb init [OPTIONS]`

**Sommaire**

 Configure un répertoire avec Weights & Biases

**Options**

| **Options** | **Description** |
| :--- | :--- |
| -p, --project | Le projet à utiliser. |
| -e, --entity | L’entité sur laquelle scope le projet. |
| --reset | Réinitialise les paramètres |
| -m, --mode | Peut être "online", "offline" ou "disabled". Par défaut |
| --help | Montre ce message et quitte. |

## wandb local

**Utilisation**

`wandb local [OPTIONS]`

**Sommaire**

Lance le containeur local W&B container \(Expérimental\)

**Options**

| **Options** | **Description** |
| :--- | :--- |
| -p, --port | Le port d’hôte auquel lier le W&B local |
| -e, --env | Vars env à passer dans wandb/local |
| --daemon | --no-daemon Exécute ou non le mode daemon |
| --upgrade | Met à jour vers la version la plus récente |
| --help | Montre ce message et quitte. |

## wandb login

**Utilisation**

`wandb login [OPTIONS] [KEY]...`

**Sommaire**

 Se connecte à Weights & Biases

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --cloud | Se connecte au cloud plutôt que de manière locale |
| --host | Se connecte à une instance spécifique de W&B |
| --relogin | Force la reconnection si déjà connecté. |
| --anonymously | Se connecte anonymement |
| --help | Montre ce message et quitte. |

## wandb offline

 **Utilisation**

`wandb offline [OPTIONS]`

**Sommaire**

 Désactive la synchro W&B

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --help | Montre ce message et quitte. |

## wandb online

 **Utilisation**

`wandb online [OPTIONS]`

**Sommaire**

 Active la synchro W&B

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --help | Montre ce message et quitte. |

## wandb pull

**Utilisation**

`wandb pull [OPTIONS] RUN`

**Sommaire**

Tire des fichiers depuis Weights & Biases

**Options**

| **Options** | **Description** |
| :--- | :--- |
| -p, --project | Le project que vous voulez télécharger. |
| -e, --entity | L’entité sur laquelle scope le listing. |
| --help | Montre ce message et quitte. |

## wandb restore

 **Utilisation**

`wandb restore [OPTIONS] RUN`

**Sommaire**

Restaure l’état de code, de config et de docker pour un run

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --no-git | Skupp |
| --branch | / --no-branch S’il faut ou non créer une branche ou un checkout détaché |
| -p, --project | Le projet sur lequel vous voulez télécharger vos données. |
| -e, --entity | L’entité sur laquelle scope le listing. |
| --help | Montre ce message et quitte. |

## wandb status

 **Utilisation**

`wandb status [OPTIONS]`

 **Sommaire**

Montre les paramètres de configuration

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --settings | / --no-settings Montre les paramètres actuels |
| --help | Montre ce message et quitte. |

## wandb sweep

 **Utilisation**

`wandb sweep [OPTIONS] CONFIG_YAML`

 **Sommaire**

Crée un balayage

**Options**

| **Options** | **Description** |
| :--- | :--- |
| -p, --project | Le projet du balayage |
| -e, --entity | Le scope d’entité pour le projet. |
| --controller | Exécute le contrôleur local |
| --verbose | Affiche l’output verbose |
| --name | Paramètre le nom du balayage |
| --program | Paramètre le programme de balayage |
| --update | Met à jour le balayage en attente |
| --help | Montre ce message et quitte. |

## wandb sync

 **Utilisation**

`wandb sync [OPTIONS] [PATH]...`

 **Sommaire**

Met en ligne un répertoire d’entraînement hors-ligne sur W&B.

**Options**

| **Options** | **Description** |
| :--- | :--- |
| --id | Le run sur lequel vous voulez mettre vos données. |
| -p, --project | Le projet sur lequel vous voulez mettre vos données. |
| -e, --entity | L’entité sur laquelle scope. |
| --include-globs | Liste séparée par des virgules de globs à inclure. |
| --exclude-globs | Liste séparée par des virgules de globs à exclure. |
| --include-online | / --no-include-online |
| Include | essai en ligne |
| --include-offline | / --no-include-offline |
| Include | essai hors-ligne |
| --include-synced | / --no-include-synced |
| Include | runs synchronisés |
| --mark-synced | / --no-mark-synced |
| Mark | exécute tel que synchronisé |
| --sync-all | Synchronise tous les runs |
| --clean | Supprime les runs synchronisés |
| --clean-old-hours | Supprime les runs créés avant ce nombre d’heures. |
| To | à utiliser avec --clean flag. |
| --clean-force | Nettoie sans prompt de confirmation. |
| --show | Nombre d’essais à afficher. |
| --help | Montre ce message et quitte. |

