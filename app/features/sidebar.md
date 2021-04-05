---
description: Comment utiliser la barre latérale et le tableau sur la page de projet
---

# Table

Sur la page de projet, nous montrons vos essais dans une barre latérale. Agrandissez cette barre latérale pour voir un tableau des mesures d’hyperparamètres et de sommaire tout au long de vos essais.

## Rechercher des noms d’essai

Nous prenons totalement en charge la recherche [regex](https://dev.mysql.com/doc/refman/8.0/en/regexp.html) des noms d’essai dans le tableau. Lorsque vous tapez une requête dans la barre de recherche, cela filtrera les essais visibles dans les graphiques sur le workspace, et cela filtrera également les lignes du tableau.

## Modifier la taille de la barre latérale

Est-ce que vous voudriez faire plus de place pour vos graphiques sur la page de projet ? Cliquez et tirez sur le rebord du titre de colonnes pour modifier la taille de la barre latérale. Vous serez toujours capable de cliquer sur l’icône d’œil pour activer et désactiver les essais sur les graphiques..

![](https://downloads.intercomcdn.com/i/o/153755378/d54ae70fb8155657a87545b1/howto+-+resize+column.gif)

## Ajouter des colonnes latérales

 Sur la page de projet, nous vous montrons les essais dans une barre latérale. Pour afficher plus de colonnes :

1. Cliquez sur le bouton en haut à droite de la barre latérale pour agrandir le tableau.
2. Sur un titre de colonne, cliquez sur le menu déroulant pour épingler une colonne.
3. Les colonnes épinglées seront disponibles dans la barre latérale lorsque vous diminuerez le tableau.

Voici une capture d’écran. J’agrandis le tableau, j’épingle deux colonnes, je diminue le tableau, puis je change la taille de la barre latérale.

![](https://downloads.intercomcdn.com/i/o/152951680/cf8cbc6b35e923be2551ba20/howto+-+pin+rows+in+table.gif)

## Sélectionnez plusieurs essais à la fois

Supprimez plusieurs essais à la fois, ou étiquetez un groupe d’essai – la sélection multiple rend plus simple l’organisation de votre tableau d’essais.

![](../../.gitbook/assets/howto-bulk-select.gif)

## Sélectionner tous les essais dans le tableau

Cliquez sur la case à cocher en haut à gauche du tableau, puis cliquez sur "Sélectionner tous les essais" \(Select all runs\) pour sélectionner chaque essai qui correspond à votre sélection courante de filtres.

![](../../.gitbook/assets/all-runs-select.gif)

##  Déplacer les essais entre les projets

Pour déplacer des essais d’un projet à un autre :

1. Agrandissez le tableau
2. Cliquez sur la case à cocher à côté des essais que vous souhaitez déplacer
3. Cliquez sur déplacer \(move\) et sélectionner le projet de destination

![](../../.gitbook/assets/howto-move-runs.gif)

## Voir les essais actifs

 Cherchez un point vert à côté du nom de vos essais – il indique qu’ils sont actifs dans ce tableau et sur les légendes de graphique.

## Cacher les essais inintéressants

Est-ce que vous souhaitez cacher les essais inaboutis ? Est-ce que les essais courts encombrent votre tableau ? Est-ce que vous ne souhaitez voir que votre travail dans un projet de groupe ? Cachez tout ce bruit avec un filtre. Certains filtres que nous recommandons :

* **Ne montrer que mon travail** filtre les essais qui sont sous votre nom d’utilisateur \(Show only my work\)
* **Cacher les essais inaboutis** enlève tous les essais marqués comme plantés depuis le tableau \(Hide crashed runs\)
* **Durée** : ajoutez un nouveau filtre et sélectionnez "durée" \(duration\) pour cacher les essais courts \(Duration\)

![](../../.gitbook/assets/image%20%2816%29.png)

##  Filtrer et supprimer les essais indésirables

 Si vous filtrez votre tableau pour n’avoir plus que les essais que vous voulez supprimer, vous pouvez sélectionner Tous \(All\) et appuyez sur supprimer pour les retirer de votre projet. La suppression d’essais est globale au projet, donc, si vous supprimez des essais d’un rapport, cela se reflétera sur le reste de votre projet.

![](../../.gitbook/assets/2020-05-13-19.14.13.gif)

##  Exporter le tableau d’essai en CSV

Exporter le tableau de tous vos essais, hyperparamètres, et mesures d’hyperparamètres en CSV avec le bouton de téléchargement.

![](../../.gitbook/assets/2020-07-06-11.51.01.gif)

