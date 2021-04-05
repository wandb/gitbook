---
description: >-
  Limites et lignes directrices appropriées pour enregistrer des données dans
  Weights & Biases
---

# Limits

### **Meilleure utilisation pour charger rapidement la page**

Pour que la page se charge rapidement dans l’IU W&B, nous vous recommandons de garder le nombre de données enregistrées dans ces limites.

* **Scalaires :** vous pouvez avoir des dizaines de milliers d’étapes et des centaines de mesures
* **Histogrammes :** nous recommandons de vous limiter à des milliers d’étapes

Si vous nous envoyez plus de données que ça, elles seront sauvegardées et retracées, mais les pages pourraient se charger plus lentement.

### **Performance de script Python**

De manière générale, vous ne devriez pas appeler `wandb.log` plus que quelques fois par seconde, ou bien wandb risque d’interférer avec la performance de votre essai d’entraînement. Nous ne faisons pas valoir d’autres limites que la limite de taux. Notre client Python fera automatiquement un arrêt exponentiel et réessaiera les requêtes qui excèdent les limites, pour que tout reste transparent pour vous. La ligne de commande affichera “Network failure” \(Défaillance réseau\). Pour les comptes gratuits, nous pouvons éventuellement vous contacter dans des cas extrêmes où l’utilisation dépasserait le seuil raisonnable.

###  **Limite de taux**

 L’API de W&B a une limite de taux, de par ses IP et sa clef API. Les nouveaux comptes sont restreints à 200 requêtes par minute. Ce taux vous permet d’exécuter approximativement 15 processus en parallèle et d’en tirer des rapports sans excéder la limite. Si le client **wandb** détecte qu’il est limité, il s’arrêtera et essaiera de renvoyer les données plus tard. Si vous avez besoin d’exécuter plus de 15 processus en parallèle, envoyez un email à [contact@wandb.com](mailto:contact@wandb.com).

###  **Limite de poids**

####  Fichiers

 Le poids de fichier maximal pour les nouveaux comptes est de 2 GB. Un essai unique peut stocker 10 GB de données. Si vous avez besoin de stocker de plus grands fichiers ou davantage de données par essai, contactez-nous à [contact@wandb.com](mailto:contact@wandb.com).

#### Mesures

Les mesures sont échantillonnées sur 1 500 points de données par défaut avant d’être affichées dans l’IU.

### Enregistrement

Lorsqu’un essai est en cours, nous traçons les 5 000 dernières lignes de vos enregistrements dans l’IU. Après la fin d’un essai, l’intégralité de l’enregistrement est archivée et peut être téléchargée depuis une page d’essai individuel.

### **Guide pour l’enregistrement**

Voici quelques lignes directrices supplémentaires pour enregistrer des données dans W&B.

* **Paramètres imbriqués** : Nous aplatissons automatiquement les paramètres imbriqués. Si vous nous envoyez un dictionnaire, nous le transformerons en un nom séparé par des points. Pour les valeurs de config, nous acceptons 3 points dans le nom. Pour les valeurs de sommaire, nous acceptons 4 points.

