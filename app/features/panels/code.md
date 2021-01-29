# Code Saving

Par défaut, nous sauvegardons seulement le dernier hash git commit. Vous pouvez activer plus de sauvegarde de code pour comparer votre code entre vos expériences de manière dynamique dans l’IU.

Depuis la version 0.8.28 de `wandb` , nous pouvons enregistrer le code de votre fichier d’entraînement principal lorsque vous appelez `wandb.init()`. Il sera synchronisé au tableau de bord et s’affichera dans un onglet sur la page d’essai, ainsi que dans le panneau de Comparateur de Code. Rendez-vous sur votre [page de paramètres](https://app.wandb.ai/settings) pour activer la sauvegarde de code par défaut.

![Here&apos;s what your account settings look like. You can save code by default.](../../../.gitbook/assets/screen-shot-2020-05-12-at-12.28.40-pm.png)

## Comparateur de code

Cliquez sur le bouton **+** dans votre workspace ou sur la page rapport pour ajouter un nouveau panneau, et sélectionner le Comparateur de code \(Code Comparer\). Faites le différentiel entre n’importe quelles deux expériences de votre projet et regardez exactement quelles lignes de codes ont été modifiées. Voici un exemple :

![](../../../.gitbook/assets/cc1.png)

## Historique de session Jupyter

Depuis la version 0.8.34 de **wandb**, notre librairie fait une sauvegarde automatique de session Jupyter. Lorsque vous appelez **wandb.init\(\)** à l’intérieur de Jupyter, nous ajoutons un crochet pour sauvegarder automatiquement un notebook Jupyter qui contient l’historique du code exécuté dans votre session actuelle. Vous pouvez trouver cet historique de session dans un navigateur de fichier d’essais sous le répertoire code :

![](../../../.gitbook/assets/cc2%20%284%29%20%284%29.png)

Cliquer sur ce fichier affichera les cellules qui ont été exécutées dans votre session, ainsi que tout output créé en appelant la méthode d’affichage iPython. Ceci vous permet de voir exactement quel code a été exécuté dans Jupyter pour un essai donné. Lorsque c’est possible, nous sauvegardons aussi la version la plus récente du notebook que vous trouverez également sous le répertoire code.

![](../../../.gitbook/assets/cc3%20%283%29%20%281%29.png)

## Différentiel Jupyter

Une dernière fonctionnalité bonus, c’est la possibilité de faire le différentiel entre les notebooks. Plutôt que de montrer le JSON brut dans notre panneau de Comparateur de code, nous extrayons chaque cellule et nous affichons toutes les lignes qui ont changé. Nous avons également des fonctionnalités passionnantes en cours de développement pour encore plus intégrer Jupyter à notre plateforme.

