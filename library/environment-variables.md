# Variables d’Environnement

Lorsque vous exécutez un script dans un environnement automatisé, vous pouvez contrôler **wandb** avec des variables d’environnement placées avant les exécutions du script ou à l’intérieur du script.

```bash
# This is secret and shouldn't be checked into version control
WANDB_API_KEY=$YOUR_API_KEY
# Name and notes optional
WANDB_NAME="My first run"
WANDB_NOTES="Smaller learning rate, more regularization."
```

```bash
# Only needed if you don't checkin the wandb/settings file
WANDB_ENTITY=$username
WANDB_PROJECT=$project
```

```python
# If you don't want your script to sync to the cloud
os.environ['WANDB_MODE'] = 'dryrun'
```

## Variables d’environnement optionnelles

Utilisez ces variables d’environnement optionnelles pour exécuter des actions comme la mise en place d’uneauthentification sur des machines à distance.

| Nom de variable | Utilisation |
| :--- | :--- |
| **WANDB\_API\_KEY** |  Définit la clef d’authentification associée à votre compte. Vous pouvez trouver votre clef sur votre [page de paramètres.](https://app.wandb.ai/settings) Elle doit être définie si wandb login n’a pas été exécuté sur la machine à distance. |
| **WANDB\_BASE\_URL** | Si vous utilisez [wandb/local](https://docs.wandb.ai/v/fr/self-hosted) , vous devez configurer cette variable d’environnement sur http://VOTRE\_IP:VOTRE\_PORT |
| **WANDB\_NAME** | Le nom humainement lisible de votre essai. S’il n’est pas défini, il sera généré de façon aléatoire pour vous. |
| **WANDB\_NOTES** | Des notes plus longues sur votre essai. Les Markdown sont autorisés et vous pouvez les éditer ultérieurement dans l’interface utilisateur. |
| **WANDB\_ENTITY** | L’entité associée à votre essai. Si vous avez lancé wandb init dans le répertoire de votre script d’entraînement, cela créera un répertoire nommé wandb et cela sauvegardera une entité par défaut qui peut être consultée dans le système de contrôle de code source. Si vous ne souhaitez pas créer ce fichier ou que vous souhaitez écraser le fichier, vous pouvez utiliser cette variable d’environnement. |
| **WANDB\_USERNAME** | Le nom d’utilisateur d’un membre de votre équipe associé à l’essai. Vous pouvez l’utiliser parallèlement à une clef API de compte de service pour permettre l’attribution d’essais automatisés à des membres de votre équipe. |
| **WANDB\_PROJECT** | Le projet associé à votre essai. Cette variable d’environnement peut aussi être configurée avec wandb init, mais elle écrasera la valeur. |
| **WANDB\_MODE** | Par défaut, cette variable est réglée sur run, ce qui sauvegarde les résultats sur wandb. Si vous voulez sauvegarder localement les métadonnées de votre essai, vous pouvez régler cette variable sur dryrun. |
| **WANDB\_TAGS** | Une liste d’étiquettes séparées par des virgules qui seront appliquées à l’essai. |
| **WANDB\_DIR** | Paramétrez cette variable en un chemin d’accès absolu pour stocker tous les fichiers générés ici plutôt que dans le répertoire wandb relatif à votre script d’entraînement. Assurez-vous que ce répertoire existe et que l’utilisateur qui exécute votre projet dispose des autorisations d’écriture requises. |
| **WANDB\_RESUME** | Par défaut, cette variable est paramétréesur never \(jamais\). Si elle est paramétréesur auto, wandb reprendra automatiquement les essais qui ont échoué. Si elle est paramétrée sur must, elle oblige l’existence de l’essai au démarrage. Si vous souhaitez générer constamment vos propres ID uniques, paramétrez cette variable sur allow et veillez à toujours configurer **WANDB\_RUN\_ID**. |
| **WANDB\_RUN\_ID** | À paramétrer sur une chaîne de caractères globalement unique \(par projet\) qui correspond à un seul essai de votre script. Ne doit pas excéder 64 caractères. Tous les caractères qui ne forment pas des mots seront convertis en tiret. Ceci peut être utilisé pour reprendre un essai existant en cas d’échec. |
| **WANDB\_IGNORE\_GLOBS** | Paramétrez cette variable sur une liste, séparée par des virgules, de fichiers globs que vous voulez ignorer. Ces fichiers ne seront pas synchronisés au cloud. |
| **WANDB\_ERROR\_REPORTING** | Paramétrez cette variable sur false pour empêcher wandb d’enregistrer des erreurs critiques dans son système de traçage d’erreur. |
| **WANDB\_SHOW\_RUN** | Paramétrez cette variable sur **true** pour ouvrir automatiquement un navigateur avec l’url de l’essai s’il est compatible avec votre système d’exploitation. |
| **WANDB\_DOCKER** | Paramétrez cette variable sur un condensé d’images Docker \(docker image digest\) pour permettre la restauration des essais. C’est configuré automatiquement avec la commande wandb docker. Vous pouvez obtenir un condensé d’images en exécutant `wandb docker my/image/name:tag --digest` |
| **WANDB\_DISABLE\_CODE** | Paramétrez cette variable sur true pour empêcher wandb de stocker une référence à votre code source |
| **WANDB\_ANONYMOUS** | Réglez sur "allow" \(permettre\), "never"\(jamais\), or "must"\(obliger\) pour permettre aux utilisateurs de créer des essais anonymes avec des URL secrètes. |
| **WANDB\_CONSOLE** | Paramétrez cette variable off pour désactiver l’enregistrement stdout / stderr. Cette variable est configurée par défaut suron dans les environnements qui le permettent. |
| **WANDB\_CONFIG\_PATHS** | Une liste de fichiers yaml séparée par des virgules à charger dans wandb.config. Voir [config](https://docs.wandb.ai/library/config#file-based-configs). |
| **WANDB\_CONFIG\_DIR** | Configurée par défaut sur ~/.config/wandb. Vous pouvez remplacer l’emplacement avec cette variable d’environnement. |
| **WANDB\_NOTEBOOK\_NAME** | Si vous exécutez votre essai sur Jupyter, vous pouvez paramétrer le nom de votre notebook avec cette variable. Nous essayons de le détecter automatiquement. |
| **WANDB\_HOST** | Paramétrez cette variable sur le nom d’hôte que vous voulez voir dans l’interface wandb, si vous ne voulez pas utiliser le système fourni de nom d’hôte. |
| **WANDB\_SILENT** | Spécifiez le nom de l’expérience pour regrouper automatiquement les essais. Avec cette configuration, tous les enregistrements seront écrits dans **WANDB\_DIR**/debug.log |
| **WANDB\_RUN\_GROUP** | Spécifiez le nom de l’expérience pour automatiquement regrouper les essais ensemble. Voir [regroupements](https://docs.wandb.ai/v/fr/library/grouping) pour plus d’infos. |
| **WANDB\_JOB\_TYPE** | Spécifiez le type de tâche, comme « entraînement » ou « évaluation » pour indiquer différents types d’essais. Voir [regroupements ](https://docs.wandb.ai/v/fr/library/grouping)pour plus d’infos. |

##  Environnements Singularity

  
Si vous exécutez des conteneurs dans [Singularity](https://singularity.lbl.gov/index.html), vous pouvez passer des variables d’environnement en faisant précéder les variables vues ci-dessus avec **SINGULARITYENV\_**. Plus de détails sur les variables d’environnement Singularity [ici](https://singularity.lbl.gov/docs-environment-metadata#environment).

## Essais sur AWS

Si vous faites des traitements par lots sur AWS, il est facile d’authentifier vos machines avec vos identifiants W&B. Obtenez votre clef API depuis votre [page de paramètres](https://app.wandb.ai/settings), et inscrivez la variable d’environnement WANDB\_API\_KEY dans les [spécifications de traitement par lots de AWS.](https://docs.aws.amazon.com/batch/latest/userguide/job_definition_parameters.html#parameters)​

##  Questions fréquentes

**Essais automatisés et comptes de service**

Si vous avez des tests automatisés ou des outils internes qui lancent des enregistrements d’exécutions sur W&B, créez un **Compte de service** **\(Service Account\)** sur la page de paramètres de votre équipe. Cela vous permettra d’utiliser une clef API de service pour vos tâches automatisées. Si vous voulez attribuer les tâchesde compte de service à un utilisateur particulier, vous pouvez utiliser les variables d’environnement WANDB\_USER\_NAME \(nom d’utilisateur\) ou WANDB\_USER\_EMAIL \(email de l’utilisateur\).

![Cr&#xE9;ez un compte de service sur la page de param&#xE8;tres de votre &#xE9;quipe pour vos traitements automatis&#xE9;s](../.gitbook/assets/image%20%2892%29.png)

Créez un compte de service sur la page de paramètres de votre équipe pour vos tâches automatisés.

### **L es variables d’environnement écrasent-elles les paramètres ajoutés sur wandb.init\(\) ?**

 Les arguments ajoutés sur wandb.init ont la priorité sur l’environnement. Vous pouvez appeler `wandb.init(dir=os.getenv("WANDB_DIR", my_default_override))` si vous voulez avoir un défaut autre que celui du système lorsque la variable d’environnement n’est pas paramétrée.

###  **Désactiver l’enregistrement**

La commande `wandb off` met en place une variable d’environnement, `WANDB_MODE=dryrun` . Ceci empêche toute donnée de se synchroniser depuis votre machine au serveur cloud wandb. Si vous avez plusieurs projets, ils arrêteront tous de synchroniser les données enregistrées sur les serveurs W&B

 Pour rendre les messages d’avertissement silencieux :

```python
import logging
logger = logging.getLogger("wandb")
logger.setLevel(logging.WARNING)
```

## **Multiples utilisateurs W&B sur des machines partagées**

 Si vous utilisez une machine partagée et qu’une autre personne est un utilisateur wandb, il est facile de vous assurer que vos essais soient toujours enregistrés sur le bon compte. Paramétrez la [variable d’environnement WANDB\_API\_KEY](https://docs.wandb.ai/v/fr/library/environment-variables) pour vous authentifier. Si vous la sourcez dans votre environnement, lorsque vous vous connectez, vous aurez les bons identifiants. Vous pouvez aussi configurer cette variable d’environnement depuis votre script.

Exécutez cette commande export `WANDB_API_KEY=X` où X est votre clef API. Une fois que vous vous êtes identifié, vous trouverez votre clef API dans [wandb.ai/authorize](https://app.wandb.ai/authorize).

