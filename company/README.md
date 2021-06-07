# Company

###  About Us – À propos de nous

### Wandb, qu’est-ce que c’est ?

Wandb est un outil de suivi des expériences d’apprentissage automatique. Nous facilitons la tâche pour quiconque souhaite garder une trace de ses expériences d’apprentissage automatique et partager les résultats avec ses collègues et pour ses projets à venir.

 Voici une vidéo d’introduction d’une minute. [Voir un exemple de projet →](https://app.wandb.ai/stacey/estuary)​

{% embed url="https://www.youtube.com/watch?v=icy3XkZ5jBk" %}

###  Comment ça fonctionne ?

Lorsque vous instrumentez votre code d’entraînement avec wandb, notre processus d’arrière-plan collecteratoutes les données utiles sur ce qui se passe tout au long des entraînements de vos modèles. Par exemple, nous pouvons garder une trace des métriques de performance de votre modèle, des hyperparamètres, des dégradés, des métriques de système, des fichiers de sortie et de votre git commit le plus récent.

### **Est-ce compliqué à mettre en place ?**

 Nous savons que la plupart des gens gardent une trace de leur entraînement avec des outils comme Emacsou Google Sheets, alors nous avons conçu wandb de manière à ce qu’il soit le plus léger possible. L’intégration devrait prendre 5 à 10 minutes, et wandb ne ralentira, ni ne fera planter votre script d’entraînement.

##  **Les avantages de wandb**

 Nos utilisateurs nous disent qu’ils tirent trois types de bénéfices de wandb :

### 1. **La visualisation des entraînements**

Certains de nos utilisateurs voient wandb comme un TensorBoard persistant. Par défaut, nous collectons les métriques de performance de modèle comme la précision et les pertes. Nous pouvons également collecter et afficher des objets Matplotlib, des fichiers de modèle, des métriques de système comme l’utilisation du GPU, et votre git commit SHA le plus récent + un fichier Patch de tous les changements depuis votre dernier commit.

 Vous pouvez également prendre des notes sur des essais individuels, qui seront enregistrées avec vos données d’entraînement. Voici un bon [exemple de projet](https://app.wandb.ai/bloomberg-class/imdb-classifier/runs/2tc2fm99/overview) tiré d’un cours que nous avons enseigné à Bloomberg.

### 2. **L’organisation et la comparaison d’essais d’entraînement en grand nombre**

La plupart des personnes qui entraînent des modèles d’apprentissage automatique essayent de très nombreuses versions de leur modèle et notre but est d’aider ces personnes à rester organisées.Vous pouvez créer des projets pour que tous vos essais se retrouvent en un seul endroit. 

Vous pouvez créer des projets pour garder tous vos essais en un seul endroit. Vous pouvez visualiser des métriques de performance à travers de nombreux essais et les filtrer, les regrouper et les libeller comme vous le souhaitez.

Un bon exemple de projet est l’[estuary project](https://app.wandb.ai/stacey/estuary) de Stacey. Dans le panneau latéral, vous pouvez activer ou désactiver l’affichage des essais sur les graphiques, ou cliquer sur un essai pour aller plus en profondeur. Tous vos essais sont sauvegardés et organisés dans un espace de travail \(workspace\) unifié pour vous.

![](../.gitbook/assets/image%20%2885%29%20%281%29%20%282%29%20%283%29%20%283%29%20%283%29%20%283%29%20%284%29%20%283%29%20%281%29%20%283%29.png)

### 3. Partage des résultats

Lorsque vous avez réalisé de nombreux essais, vous voulez généralement les organiser pour mettre en avant un certain résultat. Nos amis à Latent Space ont écrit un excellent article intitulé [ML Best Practices: Test Driven Development](https://www.wandb.com/articles/ml-best-practices-test-driven-development) \(Les meilleures pratiques en apprentissage automatique : le développement piloté par des tests\), qui parle de la manière dont ils utilisent les rapports W&B pour améliorer la productivité de leur équipe.

 Boris Dayma, un utilisateur, a écrit un rapport d’exemple public sur la [Segmentation sémantique](https://app.wandb.ai/borisd13/semantic-segmentation/reports?view=borisd13%2FSemantic%20Segmentation%20Report). Il y décrit les différentes approches qu’il a essayées, et comment elles ont fonctionné.

Nous espérons vraiment que wandb encourage les équipes d’apprentissage automatique à collaborer de manière plus productive.

Si vous voulez en apprendre davantage sur la manière dont les équipes utilisent wandb, nous avons enregistré des interviews de nos utilisateurs techniques chez [OpenAI](https://www.wandb.com/articles/why-experiment-tracking-is-crucial-to-openai) et chez [Toyota Research](https://www.youtube.com/watch?v=CaQCw-DKiO8).

##  Équipes

Si vous travaillez sur un projet d’apprentissage automatique avec des collaborateurs, nous vous simplifions le partage des résultats :

* [Équipes d’entreprise ](https://www.wandb.com/pricing): nous soutenons les petites startups et les grandes équipes d’entreprise comme OpenAI ou Toyota Research Institute. Nous avons des options de tarification flexibles pour répondre aux besoins de votre équipe, et nous prenons en charge les installations sur cloud hébergé, sur cloud privé, et sur site.
* ​[Équipes universitaires ](https://www.wandb.com/academic): nous sommes dévoués au soutien de la recherche universitaire transparente et collaborative. Si vous êtes universitaire, nous vous fournirons un accès gratuit pour des équipes afin de vous permettre de partager vos recherches dans des projets privés.

Si vous souhaitez partager un projet avec des personnes en dehors d’une équipe, cliquez sur les paramètres de confidentialité de projet \(project privacy settings\) dans la barre de navigation et paramétrez le projet sur **Public**. Toutes les personnes auxquelles vous partagerez un lien pourront voir les résultats de votre projet public.

