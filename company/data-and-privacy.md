# Data and Privacy

## Vous possédez vos données

Tout ce que vous enregistrez dans Weights & Biases est à vous, ce qui inclut vos données d’entraînement, votre code, votre configuration et vos hyperparamètres, vos mesures de sortie \(output\), votre analyse, et les fichiers de modèle sauvegardés. Vous pouvez choisir d’enregistrer, d’exporter, de publier ou de détruire n’importe lequel de ces éléments. Nous récoltons des statistiques agrégées depuis nos utilisateurs pour améliorer notre produit – il est possible que nous fassions une requête de base de données pour compter combien d’utilisateurs ont téléchargé un requirements.txt sur notre cloud qui inclue une librairie spécifique pour nous décider ou non à faire une intégration de première classe avec cette librairie. Nous traitons vos données privées, votre code source, ou vos secrets commerciaux comme confidentiels et privés, en accord avec nos [Termes d’Utilisation](https://www.wandb.com/terms) et notre [Politique de Confidentialité.](https://www.wandb.com/privacy)‌

## Enregistrement des données

Notre outil fournit la capacité d’enregistrer 4 classes primaires de données :

1. Mesures et Paramètres : \_\*\*\_ C’est la fonctionnalité core de notre outil – garder une trace des scalaires et des histogrammes que vous enregistrez avec un essai. Vous spécifiez directement ceci dans wandb.log\(\)ou vous mettez en place une intégration avec un des frameworks pris en charge.
2.  Code : Nous prenons en charge la sauvegarde du dernier git SHA et d’un diff patch, ou la sauvegarde du fichier principal de votre essai pour avoir une comparaison de code facile. Par défaut, cette fonctionnalité est désactivée, et il faut manuellement l’activer sur votre [page de paramètres](https://app.wandb.ai/settings).
3. Média : Les utilisateurs peuvent enregistrer des vidéos, des images, des textes ou des graphiques personnalisés pour visualiser comment leur modèle s’en sort sur des exemples pendant l’entraînement. Ceci est entièrement opt-in, et vous devez explicitement configurer votre script pour enregistrer cette classe de données.
4. Artefacts : Activez manuellement les enregistrements d’artefacts pour sauvegarder et versionner les datasets et les fichiers de modèle. Vous spécifiez explicitement quels fichiers vous souhaitez inclure dans les artefacts.

Toutes les données sont cryptées au repos et sont cryptées en transit dans notre offre cloud. Nous respectons toutes les requêtes de suppression de données de manière rapide et nous pouvons vous assurer qu’elles sont entièrement retirées du système.

## Auto-hébergement et cloud privé

Nous suivons les meilleures pratiques de l’industrie pour la sécurité et le cryptage de notre service hébergé cloud. Nous proposons également des [installations de cloud privé et d’auto-hébergement](https://docs.wandb.ai/self-hosted) pour les clients d’entreprise. [Contactez-nous](https://docs.wandb.ai/company/getting-help) pour en apprendre plus sur les options disponibles pour votre entreprise.

Pour une utilisation personnelle, nous avons une [Installation Docker locale](https://docs.wandb.ai/self-hosted/local) que vous pouvez exécuter sur votre propre machine.‌

## Confidentialité de projet et équipes

Par défaut, les projets Weights & Biases sont privés, ce qui signifie que les autres utilisateurs ne sont pas capables de voir votre travail. Vous pouvez éditer ce comportement par défaut sur votre [page de paramètres](https://app.wandb.ai/settings). Vous pouvez choisir de partager vos résultats avec d’autres en rendant votre projet public ou en créant une équipe pour partager des projets privés avec des collaborateurs spécifiques. Les équipes sont une fonctionnalité premium pour les entreprises. Plus d’infos sur notre [page de prix](https://www.wandb.com/pricing).

Pour soutenir l’écosystème d’apprentissage automatique, nous proposons des équipes privées gratuites aux projets universitaires et open-source. Créez un compte puis contactez-nous via [ce formulaire](https://www.wandb.com/academic) pour faire une demande d’équipe privée gratuite.

## Sauvegarde de code

 Par défaut, nous ne prenons que le dernier git SHA de votre code. De manière optionnelle, vous pouvez activer les fonctionnalités de sauvegarde de code – cela activera un panneau de comparaison code et un onglet dans l’IU pour voir la version de code qui a exécuté votre essai. Vous pouvez activer la sauvegarde de code sur votre [page de paramètres](https://app.wandb.ai/settings).

![](../.gitbook/assets/project-defaults.png)

##  Exporter des données

Vous pouvez télécharger des données sauvegardées avec Weights & Biases en utilisant notre [API d’export](https://docs.wandb.ai/ref/export-api). Nous voulons faciliter le fait de faire l’analyse personnalisée dans des notebooks, de faire un backup de vos données si vous préférez avoir une copie locale, ou de plugger vos enregistrements sauvegardés sur d’autres outils dans votre flux de travail d’apprentissage automatique.

##  Comptes liés

Si vous utilisez Google ou GitHub OAuth pour créer et vous connecter à un compte Weights & Biases, nous ne lisons ni ne synchronisons de données de vos répertoires ou de vos dossiers. Ces connexions sont purement à but d’authentification. Vous pouvez enregistrer des fichiers et du code à associer avec vos essais en utilisant les [Artefacts](https://docs.wandb.ai/artifacts)W&B.

