# Reference

##  Axe X

![S&#xE9;lection de l&#x2019;axe X](../../../../.gitbook/assets/image%20%2815%29.png)

 Vous pouvez paramétrer l’axe X d’un graphique linéaire sur n’importe quelle valeur que vous avez enregistrée avec wandb.log tant qu’elle est toujours enregistrée sous forme de nombre.

## Variables d’axe Y

 Vous pouvez paramétrer les variables de l’axe y sur n’importe quelle valeur du moment que vous enregistriez des nombres, des arrays de nombres ou un histogramme de nombres. Si vous avez enregistré plus de 1 500 points pour une variable, wandb échantillonnera seulement 1 500 points.

{% hint style="info" %}
\(i\) Vous pouvez changer la couleur des lignes de vos axes y en changeant la couleur de l’essai dans le tableau d’essai.
{% endhint %}

## Plages X et Y

Vous pouvez changer les valeurs maximum et minimum de X et Y pour le graphique.

La plage X par défaut s’étend de la plus petite valeur de votre axe X à la plus grande.

La plage Y par défaut s’étend de la plus petite valeur de vos mesures et zéro et va jusqu’à la plus grande valeur de vos mesures.

##  Essais/Groupes max

Par défaut, vous ne tracerez que 10 essais ou groupes d’essais. Ces essais seront pris du haut de votre tableau d’essai ou de set d’essai, donc, si vous triez votre tableau d’essai ou votre set d’essai, vous pouvez modifier les essais qui sont affichés.

## Légende

 Vous pouvez contrôler la légende de votre graphique pour montrer n’importe quelle valeur de config que vous avez enregistrée sur n’importe quel essai, et les métadonnées des essais, comme l’heure de création ou l’utilisateur qui a créé cet essai.

 Exemple :

${config:x} insèrera la valeur de config de x pour un essai ou groupe.

Vous pouvez paramétrer \[\[$x: $y\]\] pour afficher des valeurs spécifiques de point dans le pointeur

## Regroupements

Vous pouvez agréger tous vos essais en activant les regroupements, ou regrouper en fonction d’une variable individuelle. Vous pouvez aussi activer les regroupements en regroupant depuis le tableau, et les regroupements peupleront automatiquement le graphique.

## Lissage

 Vous pouvez paramétrer le [coefficient de lissage](https://docs.wandb.ai/library/technical-faq#what-formula-do-you-use-for-your-smoothing-algorithm) pour qu’il soit entre 0 et 1, où 0 correspond à aucun lissage et 1 au lissage maximum.

## Ignorer les données aberrantes

Ignorer les données aberrantes \(ignore outliers\) fait que le graphique paramètre le min de l’axe y sur le 5e percentile et son max sur le 95e percentile des données, plutôt que de rendre toutes les données visibles.

##  Expression

L’expression vous permet de tracer des valeurs dérivées de mesures, comme 1-précision. Pour l’instant, ne fonctionne que si vous tracez une mesure à la fois. Vous pouvez faire des expressions arithmétiques simples, +, -, _, / et %, ainsi que \*_ pour les puissances.

### Style de graphique

Sélectionnez un style pour votre graphique linéaire.

**Graphique linéaire :**

![](../../../../.gitbook/assets/image%20%285%29%20%282%29.png)

**Graphique en aires :**

![](../../../../.gitbook/assets/image%20%2835%29%20%281%29%20%282%29%20%281%29.png)

**Graphique en aires par pourcentage :**

![](../../../../.gitbook/assets/image%20%2869%29%20%284%29%20%286%29%20%282%29.png)

