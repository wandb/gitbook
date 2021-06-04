---
description: >-
  Les limites et les lignes directrices appropriées pour enregistrer des données
  sur Weights & Biases
---

# Limits

### **Les bonnes pratiques pour un chargement rapide des pages**

Pour un chargement rapide des pages dans l’interface utilisateur de W&B, nous vous recommandons de limiter le nombre de données enregistrées comme suit :

* **Scalaires :** vous pouvez avoir des dizaines de milliers d’étapes et des centaines de mesures
* **Histogrammes :** nous recommandons de vous limiter à des milliers d’étapes

Si vous dépassez ces limites, les données que vous nous envoyez seront sauvegardées et retracées, mais les pages pourraient se charger plus lentement.

### **Performance de script Python**

De manière générale, vous ne devriez effectuer tout au plus que quelques appels `wandb.log` par seconde,autrement wandb risque d’interférer avec la performance de votre essai d’entraînement. Nous ne garantissons aucune limite excédant la limitation de débit. Notre client Python appliquera automatiquement une interruption exponentielle \(exponential backoff\) et fera de nouvelles tentatives pour les requêtes qui excèdent les limites, afin que tout reste transparent pour vous. La ligne de commande affichera Network failure \(Défaillance réseau\). Pour les comptes gratuits, nous pouvons éventuellement vous contacter dans des cas extrêmes où l’utilisation dépasserait un seuil raisonnable.

###  **Limite de débit**

L’API de W&B a une limite de débit, de par ses IP et sa clef API. Les nouveaux comptes sont restreints à 200 requêtes par minute. Ce taux vous permet d’exécuter approximativement 15 processus en parallèle et d’en tirer des rapports sans réduction de débit. Si le client **wandb** détecte qu’il a atteint les limites, il s’arrêtera et essaiera de renvoyer les données plus tard. Si vous avez besoin d’exécuter plus de 15 processus en parallèle, envoyez un email à [contact@wandb.com](mailto:contact@wandb.com).

###  **Limite de poids**

####  Fichiers

Le poids de fichier maximal pour les nouveaux comptes est de 2 Go. Un seul essai peut stocker 10 Go de données. Si vous avez besoin de stocker de plus grands fichiers ou davantage de données par essai, contactez-nous à [contact@wandb.com](mailto:contact@wandb.com).

#### **Métriques**

Les métriques sont échantillonnées sur 1 500 points de données par défaut avant d’être affichées dans l’interface utiliser.

### Enregistrement

Lorsqu’un essai est en cours, nous affichons les 5 000 dernières lignes de vos enregistrements dans l’interface utilisateur. Après la fin d’un essai, l’intégralité de l’enregistrement est archivée et peut être téléchargée depuis la page d’un essai individuel.

### **Guide pour l’enregistrement**

Voici quelques directives supplémentaires pour enregistrer des données sur W&B.

* **Paramètres imbriqués** : nous aplanissons automatiquement les paramètres imbriqués. Si vous nous envoyez un dictionnaire, nous le convertirons en un nom séparé par des points. Pour les valeurs de configuration, nous acceptons 3 points dans le nom. Pour les valeurs de synthèse, nous acceptons 4 points.

