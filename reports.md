---
description: >-
  Gestion de projet et outils de collaboration pour les projets d’apprentissage
  automatique
---

# Reports

Les rapports vous permettent d’organiser vos visuels, de décrire vos découvertes, et de partager des mises à jour avec des collaborateurs.

### Cas d’utilisation

1.  **Notes** : Ajoutez un graphique avec une note pour vous.
2. **Collaboration :** Partagez vos découvertes avec vos collègues.
3. **Journal de travail :** Retracez ce que vous avez essayé, et planifiez vos prochaines étapes.

###  ​[Voir l’étude de cas OpenAI →](https://bit.ly/wandb-learning-dexterity)​

Une fois que vous avez des[ expériences sur W&B](https://app.gitbook.com/@weights-and-biases/s/docs/~/drafts/-MSS0wIG8p8DoTkCvnaD/v/francais/quickstart), visualisez facilement des résultats dans vos rapports. Voici une rapide vidéo pour en expliquer les grandes lignes.

{% embed url="https://www.youtube.com/watch?v=o2dOSIDDr1w" caption="" %}

## Collaborez sur les rapports

Une fois que vous avez sauvegardé un rapport, vous pouvez cliquer sur le bouton **Partager** pour collaborer. Assurez-vous que les paramètres de visibilité sur votre projet permettent à vos collaborateurs d’avoir accès au rapport – vous aurez besoin d’un projet ouvert ou d’un projet d’équipe pour partager un rapport que vous pouvez éditer ensemble.

 Quand vous cliquez sur éditer, vous éditerez une copie de brouillon du rapport. Ce brouillon se sauvegarde automatiquement, et lorsque vous cliquez sur **Sauvegarder dans le rapport** \(Save to report\), vous publierez vos changements sur le rapport partagé.  
Si l’un de vos collaborateurs a édité le rapport dans l’intervalle de temps, vous obtiendrez un avertissement pour vous aider à résoudre les potentiels conflits d’édition.

![](.gitbook/assets/collaborative-reports.gif)



### Commenter sur les rapports

Cliquez sur le bouton de commentaire d’un panneau dans un rapport pour ajouter un commentaire directement à ce panneau.

![](.gitbook/assets/demo-comment-on-panels-in-reports.gif)

## Grilles de panneaux

 Si vous voudriez comparer un set différent d’essais, créez une nouvelle grille de panneau. Chaque graphique de section sont contrôlés par les **Sets d’essai** \(Run Sets\) ****en bas de cette sectin.

## Sets de run

*  **Sets d’essai dynamiques** : Si vous commencez par "Tout visualiser" \(Visualize all\) et que vous filtrez ou désélectionnez des essais à visualiser, le set d’essai se mettra automatiquement à jour pour vous montrer tout nouvel essai qui correspond aux filtres.
* **Sets d’essai statiques** : Si vous commencez par "Ne rien visualiser" \(Visualize none\) et que vous sélectionnez les essais que vous voulez inclure dans votre set d’essai, vous n’obtiendrez que ces essais dans votre set de run. Aucun nouvel essai ne sera ajouté.
* **Définir des clefs :** si vous avez plusieurs Sets d’essais dans une section, les colonnes seront définies par le premier set d’essai. Pour montrer les différents clefs de différents projets, vous pouvez cliquer sur "Ajouter Grille de panneau" \(Add Panel Grid\) pour ajouter une nouvelle section de graphiques et de sets d’essais avec ce deuxième set de clefs. Vous pouvez aussi dupliquer une section de grille.

## Exporter les rapports

Cliquez sur le bouton téléchargement pour exporter votre rapport sous forme de fichier LaTeX zippé. Consultez le README.md dans votre dossier téléchargé pour comprendre comment convertir ce fichier en PDF. Il est facile d’envoyer un fichier zip vers [Overleaf](https://www.overleaf.com/) pour éditer le LaTeX.

## Rapports sur différents projets

Comparez des essais de deux projets différents avec les rapports sur différents projets \(cross-project reports\). Utilisez le sélecteur de projet dans le tableau de set d’essais pour sélectionner un projet.

![](.gitbook/assets/how-to-pick-a-different-project-to-draw-runs-from.gif)

Les visuels de cette section prennent des colonnes depuis le premier set d’essai actif. Si vous ne voyez pas les mesures que vous recherchez dans le graphique linéaire, assurez-vous que le premier set d’essai sélectionné dans la section a cette colonne de disponible. Cette fonctionnalité prend en charge les données historiques sur des lignes de séries chronologiques, mais nous ne prenons pas en charge l’extraction de différentes mesures de sommaire depuis différents projets – donc, un nuage de points ne fonctionnerait pas pour des colonnes qui sont seulement enregistrées dans un autre projet.

Si vous avez vraiment besoin de comparer des essais de deux projets et que les colonnes ne fonctionnent pas, ajoutez une étiquette aux essais dans un des projets, puis déplacez ces essais dans l’autre projet. Vous serez toujours capable de filtrer les essais de chaque projet, mais vous aurez toutes les colonnes pour les deux sets d’essais qui seront disponibles dans le rapport.

### Lien pour rapport en lecture seule

 Partagez un lien en lecture seule vers un rapport qui est dans un projet privé ou dans un projet d’équipe.

![](.gitbook/assets/share-view-only-link.gif)

###  Envoyer un graphique à un rapport

Envoyez un graphique depuis votre workspace à un rapport pour garder une trace de vos progrès. Cliquez sur le menu déroulant sur le graphique ou le panneau que vous souhaitez copier dans un rapport et cliquez sur **Ajouter à rapport** \(Add a report\) pour sélectionner le rapport de destination.

![](.gitbook/assets/demo-export-to-existing-report%20%281%29%20%282%29%20%283%29%20%281%29.gif)

##  FAQ des Rapports

### Envoyer un CSV vers un rapport

 Actuellement, si vous souhaitez envoyer un CSV local vers un rapport, vous pouvez le faire via le format `wandb.Table`. Chargez le CSV dans votre script Python et enregistrez-le dans un objet `wandb.Table` pour vous permettre d’avoir un rendering des données comme un tableau dans un rapport.

###  Rafraîchir les données

 Faites un nouveau chargement de page pour rafraîchir les données dans un rapport et obtenir les derniers résultats depuis vos essais actifs. Les Workspaces chargent automatiquement des données nouvelles si vous avez l’option **Auto-refresh** activée \(disponible dans le menu déroulant dans le coin supérieur droit de votre page\). L’auto-refresh ne s’applique pas aux rapports, donc les données ne se rafraîchiront pas tant que vous ne ferez pas un nouveau chargement de page.

