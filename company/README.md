# Company

###  Wandb, qu’est-ce que c’est ?

Wandb est un outil de suivi des expériences pour l’apprentissage automatique. Nous rendons facile pour n’importe quelle personne qui fait de l’apprentissage automatique de garder une trace de ses expériences et de partager les résultats avec ses collègues et son futur soi.

Voici une vidéo d’aperçu d’1 minute. [Voir un exemple de projet →](https://app.wandb.ai/stacey/estuary)

{% embed url="https://www.youtube.com/watch?v=icy3XkZ5jBk" %}

###  Comment ça fonctionne ?

Lorsque vous instrumentez votre code d’entraînement avec wandb, notre processus de fond récoltera toutes les données utiles sur ce qu’il se passe tandis que vous entraînez vos modèles. Par exemple, nous pouvons garder une trace des mesures de performance de votre modèle, des hyperparamètres, des dégradés, des mesures de système, des fichiers output, et de votre git commit le plus récent.

### C’est compliqué à mettre en place ?

 Nous savons que la plupart des gens gardent une trace de leur entraînement avec des outils comme emacs ou Google Sheets, alors nous avons élaboré wandb pour qu’il soit le plus léger possible. L’intégration devrait prendre 5-10 minutes, et wandb ne ralentira pas et ne fera pas planter votre script d’entraînement.

##  Bénéfices de wandb

 Nos utilisateurs nous disent qu’ils tirent trois types de bénéfices de wandb :

### 1. Visualisation de l’entraînement

Certains de nos utilisateurs pensent à wandb comme à un "TensorBoard persistant". Par défaut, nous récoltons les mesures de performance de modèle comme la précision et les pertes. Nous pouvons aussi récolter et afficher des objets matplotlib, des fichiers de modèle, des mesures de système comme l’utilisation GPU, et votre git commit SHA le plus récent + un fichier de patch de tous les changements depuis votre dernier commit.

Vous pouvez également prendre des notes sur des essais individuels, qui seront enregistrées avec vos données d’entraînement. Voici un [exemple de projet](https://app.wandb.ai/bloomberg-class/imdb-classifier/runs/2tc2fm99/overview) pertinent tiré d’un cours que nous avons enseigné à Bloomberg.

### 2. Organisation et comparaisons de nombreux essais d’entraînement

La plupart des personnes qui entraînent des modèles d’apprentissage automatique essayent de très nombreuses versions de leur modèle et notre but est d’aider ces personnes à rester organisées.Vous pouvez créer des projets pour que tous vos essais se retrouvent en un seul endroit. 

Vous pouvez visualiser des mesures de performance à travers de nombreux essais et filtrer, regrouper, et les libeller comme vous le souhaitez.Un bon exemple de projet est l’[estuary project](https://app.wandb.ai/stacey/estuary) de Stacey. Dans la barre latérale, vous pouvez activer ou désactiver l’affichage d’essais sur les graphiques, ou cliquer sur un essai pour aller plus en profondeur. Tous vos essais sont sauvegardés et organisés dans un workspace unifié pour vous.

![](../.gitbook/assets/image%20%2885%29%20%281%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29%20%284%29%20%283%29%20%283%29.png)

### 3. Partage des résultats

Une fois que vous avez réalisé de nombreux essais, vous avez souvent envie de les organiser pour montrer un certain résultat. Nos amis à Latent Space ont écrit un bel article intitulé [ML Best Practices: Test Driven Development](https://www.wandb.com/articles/ml-best-practices-test-driven-development)\(Meilleures pratiques en apprentissage automatique : le développement mené par les tests\), qui parle de la manière dont ils utilisent les rapports W&B pour améliorer la productivité de leur équipe.

Boris Dayma, un utilisateur, a écrit un rapport d’exemple public sur la [Segmentation Sémantique](https://app.wandb.ai/borisd13/semantic-segmentation/reports?view=borisd13%2FSemantic%20Segmentation%20Report). Il traite plusieurs approches différentes qu’il a essayées, et comment elles ont fonctionné.

Nous souhaitons vraiment que wandb encourage les équipes d’apprentissage automatique à collaborer de manière plus productive.

Si vous voulez en apprendre plus sur la manière dont les équipes utilisent wandb, nous avons enregistré des interviews avec nos utilisateurs techniques chez [OpenAI](https://www.wandb.com/articles/why-experiment-tracking-is-crucial-to-openai) et chez [Toyota Research](https://www.youtube.com/watch?v=CaQCw-DKiO8).

##  Équipes

Si vous travaillez sur un projet d’apprentissage automatique avec des collaborateurs, nous rendons facile le partage des résultats :

*  [Équipes d’Entreprise ](https://www.wandb.com/pricing): Nous soutenons les petites startups et les grandes équipes d’entreprise comme OpenAI ou le Toyota Research Institute. Nous avons des options de prix flexibles pour répondre aux besoins de votre équipe, et nous prenons en charge les installations sur cloud hébergé, sur cloud privé, et sur site.
* [Équipes Universitaires ](https://www.wandb.com/academic): Nous sommes dévoués au soutien de la recherche universitaire transparente et collaborative. Si vous êtes universitaire, nous vous donnerons un accès gratuit aux équipes pour partager vos recherches dans des projets privés.

Si vous aimeriez partager un projet avec des personnes en dehors d’une équipe, cliquez sur les paramètres de confidentialité de projet \(project privacy settings\) dans la barre de navigation et paramétrez le projet sur "Public". Toutes les personnes avec lesquelles vous partagerez un lien seront capables de voir vos résultats de projet public.

