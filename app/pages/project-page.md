---
description: >-
  Comparez les versions de votre modèle, explorez les résultats dans un espace
  de travail isolé, et exportez vos découvertes dans un rapport pour sauvegarder
  vos notes et vos visuels
---

# Project Page

Le projet **Workspace** \(espace de travail\) vous donne un bac à sable personnel dans lequel comparer des expériences. Utilisez des projets pour organiser des modèles qui peuvent être comparés, en travaillant sur le même problème avec différentes architectures, hyperparamètres, datasets, prétraitements, etc.

Onglets de page de projet :

1. [**Vue d’ensemble** ](https://docs.wandb.ai/app/pages/project-page#overview-tab): aperçu de votre projet
2. [**Workspace**](project-page.md#workspace-tab): bac à sable de visualisation personnel
3. [**Tableau**](https://docs.wandb.ai/app/pages/project-page#table-tab)**:**  vue globale de tous vos essais
4.  [**Rapports**](https://docs.wandb.ai/app/pages/project-page#reports-tab): aperçus sauvegardés de vos notes, vos essais, et vos graphiques
5.  ****[**Balayages**](https://docs.wandb.ai/app/pages/project-page#sweeps-tab) ****: exploration et optimisation automatiques.

##  Onglet de vue d’ensemble

* **Nom de projet** : cliquez pour éditer le nom du projet 
* **Description de projet** : cliquez pour éditer la description du projet et ajouter des notes
* **Suppression de projet** : cliquer sur les trois petits points en haut à droite pour supprimer un projet
* **Confidentialité du projet** : éditez les personnes qui peuvent voir les essais et les rapports – cliquez sur l’icône de cadenas
* **Calcul total** : nous ajoutons tous les temps d’essais de votre projet pour obtenir ce total
* **Restaurez des essais** : Cliquez sur le menu déroulant et cliquez sur "Undelete all runs" pour restaurez les essais supprimés de votre projet.

 [Voir un exemple en direct →](https://app.wandb.ai/example-team/sweep-demo/overview)

![](../../.gitbook/assets/image%20%2829%29%20%281%29%20%282%29%20%284%29%20%281%29.png)

![](../../.gitbook/assets/undelete.png)

## Onglet de Workspace

**Barre latérale d’essais** : Liste de tous les essais de votre projet

*  **Menu …** : passez sur une ligne dans la barre latérale pour voir le menu apparaître sur le côté gauche. Utilisez ce menu pour renommer un essai, supprimer un essai, ou arrêter un essai en cours.
* **Icône de visibilité :** cliquez sur l’œil pour activer ou désactiver les essais sur graphiques
* **Couleur :** change la couleur de l’essai en une autre tirée de nos presets ou en une couleur personnalisée
* **Recherche : r**echerchez les essais par nom. Filtre aussi les essais visibles sur les graphiques linéaires.
* **Filtre** : utilisez le filtre dans la barre latérale pour réduire le champ des essais visibles
* **Groupe : s**électionnez une colonne de config pour regrouper vos essais de manière dynamique, par exemple par architecture. Les regroupements font s’afficher les graphiques avec une ligne le long de la valeur moyenne, et une région ombrée pour la variance des points sur le graphique.
* **Trier :** Choisissez une valeur selon laquelle trier vos essais, par exemple les essais avec le moins de perte ou le plus de précision. Trier affectera quels essais seront affichés sur les graphiques.
* **Bouton agrandir :** agrandit la barre latérale en tableau complet
* **Compte d’essais :** le nombre entre parenthèses tout en haut est le nombre total d’essais pour ce projet. Le nombre \(N visualisé\) est le nombre d’essais qui ont l’œil activé et qui sont disponibles à la visualisation dans chaque graphique. Dans l’exemple ci-dessous, les graphiques ne montrent que les 10 premiers essais parmi 183. Éditez un graphique pour augmenter le nombre maximum d’essais visibles.

**Disposition des panneaux** : Utilisez cet espace libre pour explorer les résultats, ajouter et retirer des graphiques, et comparer des versions de vos modèles en vous basant sur différentes mesures

[Voir un exemple en direct →](https://app.wandb.ai/example-team/sweep-demo)

![](../../.gitbook/assets/image%20%2838%29%20%282%29%20%283%29%20%283%29%20%282%29.png)

###  Rechercher des essais

Cherchez un essai avec son nom dans la barre latérale. Vous pouvez utiliser regex pour filtrer les essais qui sont visibles. La barre de recherche affecte les essais qui sont montrés sur le graphique. Voici un exemple :

![](../../.gitbook/assets/2020-02-21-13.51.26.gif)

### Ajouter une section de panneaux

Cliquez sur la section de menu déroulant et cliquer sur "Ajouter section" pour créer une nouvelle section de panneaux. Vous pouvez renommer les sections, les tirer pour les réorganiser, et agrandir ou réduire des sections.

Chaque section possède des options dans le coin en haut à droite :

*   **Passer en disposition personnalisée :** Cette disposition personnalisée vous permet de changer les tailles de vos panneaux de manière individuelle.
* **Passer en disposition standard :** La disposition standard vous permet de changer les tailles de tous les panneaux de la section d’un seul coup, et vous donne une pagination.
* **Ajouter section :** Ajoute une section au-dessus ou en-dessous depuis le menu déroulant, ou cliquez sur le bouton en bas de la page pour ajouter une nouvelle section.
*  **Renommer section :** Change le titre de votre section.
* **Exportez section dans rapport :** Sauvegarde cette section de panneaux dans un nouveau rapport.
* **Supprimer section :** Retire la section entière et tous les graphiques. Peut être annulé avec le bouton annuler en bas de la page, dans la barre de workspace.
* **Ajouter panneau :** Cliquez sur le bouton plus pour ajouter un panneau à la section.

![](../../.gitbook/assets/add-section.gif)

### Déplacer des panneaux entre des sections

Cliquez et déplacez des panneaux pour les organiser en sections. Vous pouvez aussi cliquer sur le bouton "Move" \(Déplacer\) en haut à droite d’un panneau pour sélectionner une section vers laquelle déplacer ce panneau.

![](../../.gitbook/assets/move-panel.gif)

### Modifier la taille des panneaux

**Disposition standard :** Tous les panneaux conservent la même taille, et il y a des pages de panneaux. Vous pouvez modifier la taille des panneaux en cliquant et en déplaçant le coin inférieur droit. Changer la taille d’une section en cliquant et en déplaçant le coin inférieur droit de cette section.

**Disposition personnalisée :** Tous les panneaux ont une taille qui leur est propre, et il n’y a pas de pages.

![](../../.gitbook/assets/resize-panel.gif)

### Rechercher des mesures

Utilisez la barre de recherches dans le workspace pour filtrer vos panneaux. Cette recherche correspond aux titres de panneaux, qui sont par défaut les noms des mesures visualisées.

![](../../.gitbook/assets/search-in-the-workspace.png)

## Onglet Tableau

Utilisez ce tableau pour filtrer, regrouper, et trier vos résultats.

 [Voir un exemple en direct →](https://app.wandb.ai/example-team/sweep-demo/table?workspace=user-carey)

![](../../.gitbook/assets/image%20%2886%29.png)

## Onglet Rapports

Visualisez tous les instantanés de vos résultats à un seul endroit, et partagez vos découvertes avec votre équipe.

![](../../.gitbook/assets/reports-tab.png)

##  Onglet Balayages

Commencez un nouveau balayage pour votre projet.

![](../../.gitbook/assets/sweeps-tab.png)

## Questions fréquentes

### Réinitialiser le workspace

Si vous voyez une erreur comme celle ci-dessous sur votre page de projet, voici comment réinitialiser votre workspace.`"objconv: "100000000000" overflows the maximum values of a signed 64 bits integer"`

Ajoutez **?workspace=clear à la fin de l’URL et appuyez sur Entrée. Cela devrait vous emmener jusqu’à une version nettoyée du workspace de votre page de projet.**

### Supprimer des projets

Vous pouvez supprimer votre projet en cliquant sur les trois petits points sur la droite de l’onglet Vue d’ensemble.

![](../../.gitbook/assets/howto-delete-project.gif)

###  Paramètres de confidentialité

Cliquez sur le cadenas dans la barre de navigation en haut de la page pour changer les paramètres de confidentialité du projet. Vous pouvez éditer les personnes qui peuvent voir ou ajouter des essais à votre projet. Ces paramètres incluent tous les essais et les rapports de ce projet. Si vous aimeriez partager vos résultats avec seulement quelques personnes, vous pouvez créer une [équipe privée](https://docs.wandb.ai/app/features/teams).

![](../../.gitbook/assets/image%20%2879%29.png)

### Supprimer un projet vide

Supprimer un projet sans aucun essai en cliquant sur le menu déroulant et sélectionnez "Delete project".

![](../../.gitbook/assets/image%20%2866%29.png)

