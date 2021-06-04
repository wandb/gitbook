# FAQ technique

### **Quel impact ça a mon processus d’entraînement ?**

 Lorsque `wandb.init()` est appelé depuis votre script d’entraînement, un appel API est fait pour créer un objet d’exécution \(run object\) sur nos serveurs. Un nouveau processus débute pour diffuser et collecter les métriques, maintenant ainsi tous fils et logiques en dehors de votre processus primaire. Votre script s’exécute normalement et écrit dans des fichiers locaux, tandis que le processus séparé envoie ces informations à nos serveurs, avec des métriques des systèmes. Vous pouvez toujours désactiver la diffusion d’informations en exécutant wandb off depuis votre dossier d’entraînement, ou en configurant la variable d’environnement WANDB\_MODE sur "dryrun".

### **Un plantage wandb peut-il générer un plantage de mon essai d’entraînement ?**

Il est extrêmement important pour nous de ne jamais interférer avec vos essais d’entraînement. Nous exécutons wandb dans un processus séparé pour nous assurer que, si jamais wandb plante, votre entraînement continuera à s’exécuter. Si la connexion Internet est coupée, wandb continuera à réessayer d’envoyer les données à wandb.com.

### Est-ce que wandb va ralentir mon entraînement ?

Wandb devrait avoir un effet négligeable sur vos performances d’entraînement, si vous l’utilisez normalement. Une utilisation normale de wandb signifie enregistrer moins d’une fois par seconde et enregistrer moins de quelques mégabytes de données à chaque étape. Wandb s’exécute dans un processus séparé et les appels de fonction ne se bloquent pas, ce qui signifie qu’une courte panne de réseau ou des difficultés intermittentes de lecture ou d’écriture sur le disque ne devraient pas affecter votre performance. Il est possible d’enregistrer rapidement une énorme quantité de données, et si vous faites cela, vous risquez de créer des problèmes d’IOPS sur votre disque. Si vous avez des questions, n’hésitez pas à nous contacter.

###  Puis-je utiliser wandb hors-ligne ?

Si vous faites de l’entraînement sur une machine hors-ligne et que vous souhaitez télécharger vos résultats sur nos serveurs après coup, nous avons une fonctionnalité pour vous :

1.  Régler la variable d’environnement `WANDB_MODE=dryrun` pour sauvegarder localement vos mesures, sans avoir besoin d’internet.
2. Lorsque vous êtes prêt, exécutez `wandb init` dans votre répertoire pour paramétrer le nom du projet.
3. Exécutez `wandb sync VOTRE_DOSSIER_RUN` pour transférer les mesures à notre service cloud et voir vos résultats dans notre application web dédiée.

### Est-ce que votre outil retrace ou stocke les données d’entraînement ?

 Vous pouvez ajouter un SHA ou tout autre identifiant unique sur `wandb.config.update(...)` pour associer un jeu de données à un essai d’entraînement. W&B ne stocke aucune donnée, à moins que `wandb.save` ne soit appelé avec le nom de fichier local.

### À quelle fréquence les mesures systèmes sont-elles prises ?

Par défaut, les métriques sont collectées toutes les 2 secondes, et une moyenne est réalisée sur une période de 30 secondes. Si vous avez besoin de métriques à plus haute résolution, envoyez-nous un e-mail à [contact@wandb.com](mailto:contact@wandb.com).

### Est-ce que ça ne fonctionne que pour Python ?

Pour l’instant, la bibliothèque ne fonctionne qu’avec les projets Python 2.7+ & 3.6+. L’architecture mentionnée plus haut devrait nous permettre d’intégrer facilement d’autres langages. Si vous avez besoin d’un suivi pour d’autres langages, envoyez-nous une note à [contact@wandb.com](mailto:contact@wandb.com).

### Est-ce que je peux enregistrer simplement les mesures, sans code ni exemples de dataset ?

#### Exemples de dataset

Par défaut, nous n’enregistrons aucun de vos exemples dataset. Vous pouvez explicitement activer cette fonctionnalité pour voir des prédictions d’exemple dans notre interface web.

#### Enregistrement de code

Il y a deux manières de désactiver l’enregistrement de code:

1. Paramétrez **WANDB\_DISABLE\_CODE** sur **true** pour désactiver tout traçage de code. Nous ne récupérons pas de SHA git ou de patch diff.
2.  1. Paramétrez **WANDB\_IGNORE\_GLOBS** sur **\*.patch** pour désactiver la synchronisation du patch diff sur nos serveurs. Vous l’aurez toujours localement, et vous pourrez l’appliquer avec la commande[ wandb restore.](https://docs.wandb.ai/v/fr/library/restore)

###  **L’enregistrement bloque-t-il mon entraînement ?**

« Est-ce que cette fonction d’enregistrement est lente ? Je ne veux pas être dépendant du réseau pour envoyer les résultats à vos serveurs et seulement après continuer avec mes opérations locales. »

 Appeler **wandb.log** génère l’écriture d’une ligne dans un fichier local ; ça ne bloque pas les autres appels réseaux. Lorsque vous appelez wandb.init , nous lançons un nouveau processus sur la même machine que celle qui surveille les changements de fichier système et communique à notre service web de manière asynchrone depuis votre processus d’entraînement.

###  Quelle formule utilisez-vous pour votre algorithme de lissage ?

Nous utilisons la même formule de la moyenne mobile exponentielle que TensorBoard. Vous pouvez retrouver une explication détaillée ici :[https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar](https://stackoverflow.com/questions/42281844/what-is-the-mathematics-behind-the-smoothing-parameter-in-tensorboards-scalar).

### En quoi W&B est-il différent de TensorBoard ?

 Nous apprécions les utilisateurs de Tensorboard, et nous avons une [intégration TensorBoard](https://docs.wandb.ai/v/fr/integrations/tensorboard) ! Nous aspirons à améliorer les outils de suivi d’expérience pour tout le monde. Lorsque nos cofondateurs ont commencé à travailler sur W&B, ils ont voulu construire un outil pour les utilisateurs frustrés de TensorBoard qui travaillaient chez OpenAI. Voici quelques points sur lesquels nous avons concentré nos efforts d’amélioration :

1. **Reproduire les modèles** : Weights & Biases est efficace pour expérimenter, explorer et reproduire les modèles ultérieurement. Nous enregistrons non seulement les métriques, mais aussi les hyperparamètres et la version du code, et nous pouvons sauvegarder les checkpoints de votre modèle pour vous pour que votre projet soit reproductible.
2. **Organisation automatique** : si vous passez un projet à un collaborateur ou que vous partez en vacances, W&B facilite la visualisation de tous les modèles que vous avez déjà essayés, ce qui vous évite de passer des heures à réexécuter d’anciennes expériences.
3.  **Intégration rapide et flexible** : ajoutez W&B à votre projet en 5 minutes. Installez notre package Python gratuitement en open-source et ajoutez quelques lignes à votre code, et à chaque fois que vous essaierez votre modèle, vous aurez d’excellents enregistrements de données et de métriques.
4. **Tableau de bord centralisé permanent** : quel que soit l’emplacement où vous souhaitez entraîner vos modèles, que ce soit sur votre ordinateur local, dans la grappe de serveurs \(cluster\) de votre Lab, ou pour des instances ponctuelles dans le cloud, nous vous fournissons le même tableau de bord centralisé. Vous n’avez pas besoin de passer votre temps à copier et à organiser des fichiers TensorBoard depuis différentes machines.
5. **Tableau puissant** : recherchez, filtrez, organisez et regroupez vos résultats depuis différents modèles. Il facilite la visualisation de milliers de versions de modèle et la recherche de ceux qui offrent les meilleures performances dans différentes tâches. TensorBoard n’est pas conçu pour bien fonctionner sur de grands projets.
6. **Des outils dédiés à la collaboration** : utilisez W&B pour organiser des projets complexes d’apprentissage automatique. Il est facile de partager un lien vers W&B, et vous pouvez utiliser la fonction d’équipe privée pour que tout le monde puisse envoyer des résultats sur un projet en commun. Nous soutenons aussi la collaboration via des rapports – ajoutez des visuels interactifs et décrivez votre travail dans un Markdown. C’est une excellente manière de maintenir un journal de bord, partager vos résultats avec votre superviseur, ou de présenter vos résultats à votre Lab.

Commencez en créant un [compte personnel gratuit →](http://app.wandb.ai/)

### Comment configurer le nom de mon essai dans mon code d’entraînement ?

Tout en haut de votre script d’entraînement, lorsque vous appelez wandb.init, passez le nom d’une expérience, comme ceci :`wandb.init(name="my awesome run")`

### Comment obtenir le nom d’essai aléatoire dans mon script ?Comment obtenir le nom d’essai aléatoire dans mon script ?

 Appelez `wandb.run.save()` puis obtenez le nom avec `wandb.run.name` .

###  Est-ce qu’il y a un package anaconda ?

Nous n’avons pas de package anaconda, mais vous devriez pouvoir installer wandb en utilisant :

```text
conda activate myenv
pip install wandb
```

Si vous rencontrez des difficultés avec cette installation, merci de nous le signaler. Vous trouverez quelques lignes directrices utiles dans cette [documentation d’Anaconda sur la gestion de packages](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html).

### **Comment empêcher wandb d’écrire dans mon terminal ou dans les sorties de mon notebook Jupyter ?**

Paramétrez la variable d’environnement [WANDB\_SILENT](https://docs.wandb.ai/v/fr/library/environment-variables).

Dans un notebook :

```text
%env WANDB_SILENT true
```

Dans un script python :

```text
os.environ["WANDB_SILENT"] = "true"
```

### **Comment puis-je arrêter une tâche sur wandb ?**

Appuyez sur CTRL + D sur votre clavier pour arrêter un script qui est instrumenté avec wandb.

### Comment gérer les problèmes de réseau ?

Si vous voyez des erreurs SSL ou réseau :`wandb: Network error (ConnectionError), entering retry loop.` \(Erreur reseau \(ErreurConnexion\), début de la boucle de nouvel essai\). Vous pouvez essayer différentes approches pour résoudre ce problème :

1. Améliorez votre certificat SSL. Si vous exécutez votre script sur un serveur Ubuntu, utilisez `update-ca-certificates`. Nous ne pouvons pas synchroniser les enregistrements d’entraînement sans un certificat SSL valide, parce que ça représente une faille potentielle de sécurité.
2. Si votre réseau n’est pas stable, lancez votre entraînement en [mode hors-ligne](https://docs.wandb.com/resources/technical-faq#can-i-run-wandb-offline) et synchronisez les fichiers en nous les envoyant depuis une machine qui a un bon accès internet.
3. Essayez d’exécuter [W&B Local](https://docs.wandb.ai/v/fr/self-hosted/local), qui opère sur votre machine et ne synchronise pas les fichiers à nos serveurs cloud.

**SSL CERTIFICATE\_VERIFY\_FAILED :** cette erreur peut être due au pare-feu de votre entreprise. Vous pouvez configurer des CA \(Autorités de certification\) locales, et ensuite utiliser :

`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt`

### **Que se passe-t-il si la connexion Internet plante lorsque j’entraîne un modèle ?**

Si notre bibliothèque n’est pas capable de se connecter à Internet, elle entrera une boucle de nouvel essai et continuera d’essayer d’envoyer les métriques jusqu’à ce que le réseau soit rétabli. Pendant ce temps, votre programme est capable de continuer à tourner.

Si vous devez faire un essai sur une machine non connectée à Internet, vous pouvez utiliser `WANDB_MODE=dryrun` afin que les métriques soient stockées uniquement en local, sur votre disque dur. Vous pourrez appeler wandb sync DIRECTORY \(répertoire\) plus tard pour que les données soient envoyées à notre serveur.

### Puis-je enregistrer des mesures sur deux échelles de temps différentes ? \(Par exemple, précision d’entraînement par lot et précision de validation par epoch.\)

 Oui, vous pouvez le faire en enregistrant plusieurs métriques, puis en les configurant en tant que valeur d’axe x. Ainsi, vous pouvez appeler `wandb.log({'train_accuracy': 0.9, 'batch': 200})`\(précision d’entraînement par lot\) dans une étape, et appeler `wandb.log({'val_acuracy': 0.8, 'epoch': 4})` \(précision de validation par epoch\) dans une autre étape.

### Comment enregistrer une mesure qui ne change pas au fil du temps, comme la précision d’évaluation finale ?

L’utilisation de wandb.log\({'final\_accuracy': 0.9} fonctionnera très bien pour cela. Par défaut, wandb.log\({'final\_accuracy'}\) mettra à jour wandb.settings\['final\_accuracy'\] , qui est la valeur indiquée sur le tableau des essais.

### Comment enregistrer des mesures additionnelles après la fin d’un essai ?

 Il y a plusieurs manières de le faire.

Pour les flux de travaux compliqués, nous vous recommandons d’utiliser plusieurs essais et de configurer le paramètre de groupe dans [wandb.init ](https://docs.wandb.ai/v/fr/library/init)sur une valeur unique dans tous les processus qui sont exécutés comme des parties d’une seule expérience. Le [tableau des essais](https://docs.wandb.ai/v/fr/app/pages/run-page) regroupera automatiquement le tableau par ID de groupe et les visualisations fonctionneront comme prévu. Cela vous permettra d’effectuer de multiples expériences et essais d’entraînement pendant que des processus séparés enregistrent tous les résultats à un seul endroit.

Pour les flux de travaux plus simples, vous pouvez appeler wandb.init avec resume=True et id=UNIQUE\_ID puis, plus tard, appeler de nouveau wandb.init avec la même id=UNIQUE\_ID. Ensuite, vous pouvez enregistrer normalement avec [wandb.log ](https://docs.wandb.ai/v/fr/library/log)ou wandb.summary et les valeurs de votre essai se mettront à jour.

Vous pouvez utiliser l’[API](https://docs.wandb.ai/v/fr/ref/export-api) à tout moment pour ajouter des métriques d’évaluations supplémentaires.

### Quelle est la différence entre .log\(\) et .summary ?

La synthèse \(summary\) est la valeur qui est affichée dans le tableau, alors que le log sauvegardera toutes les valeurs pour le traçage ultérieur de graphiques.

 Par exemple, vous pourriez souhaiter appeler `wandb.log` à chaque fois que la précision change. Normalement, vous pouvez simplement utiliser .log . `wandb.log()` mettra également à jour la valeur de synthèse par défaut, à moins que vous n’ayez paramétré cette valeur manuellement pour cette métrique.

Le nuage de points et le graphique de coordonnées parallèles utiliseront également la valeur de synthèse, tandis que le graphique linéaire tracera toutes les valeurs configurées par .log.

La raison pour laquelle nous avons les deux, c’est que certaines personnes préfèrent régler le sommaire manuellement parce qu’ils veulent que le sommaire reflète, par exemple, la précision optimale plutôt que la dernière précision enregistrée.

### **Comment installer la bibliothèque Python de wandb dans des environnements sans gcc ?**

  Si vous essayez d’installer `wandb` et que vous voyez cette erreur :

```text
unable to execute 'gcc': No such file or directory
error: command 'gcc' failed with exit status 1
```

Vous pouvez installer psutil directement depuis une roue prédéfinie. Vous trouverez votre version de Python et de système d’exploitation \(OS\) ici : [https://pywharf.github.io/pywharf-pkg-repo/psutil](https://pywharf.github.io/pywharf-pkg-repo/psutil)  

Par exemple, pour installer psutil sur python 3.8 sous linux :

```text
pip install https://github.com/pywharf/pywharf-pkg-repo/releases/download/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl/psutil-5.7.0-cp38-cp38-manylinux2010_x86_64.whl#sha256=adc36dabdff0b9a4c84821ef5ce45848f30b8a01a1d5806316e068b5fd669c6d
```

Après l’installation de psutil, vous pouvez installer wandb avec `pip install wandb`

### **Comment wandb diffuse les enregistrements et écrit sur le disque ?**

W&B place les événements en file d’attente dans la mémoire, mais il [les écrit également sur le disque](https://github.com/wandb/client/blob/7cc4dd311f3cdba8a740be0dc8903075250a914e/wandb/sdk/internal/datastore.py) de façon asynchrone pour gérer les échecs et pour les cas de WANDB\_MODE=offline où vous pourrez synchroniser les données ultérieurement à leur enregistrement.

Sur votre terminal, vous pouvez voir un chemin d’accès vers le répertoire local de l’essai. Ce répertoire contiendra un fichier .wandb qui est la mémoire de données susmentionnée. Si vous enregistrez également des images, nous les écrivons dans la section Media/Images de ce répertoire avant de les télécharger dans notre stockage cloud.

### **Comment obtenir plusieurs graphiques avec une sélection de différents essais ?**

Avec les rapports wandb, la procédure est comme suit :

* Créez de multiples grilles de panels.
* Ajoutez des filtres pour filtrer les sets d’essais de chaque grille de panels. Cela vous permettra de sélectionner les essais que vous voulez décrire respectivement dans chaque panel.
* Créez les graphiques que vous voulez dans les grilles de panels.

