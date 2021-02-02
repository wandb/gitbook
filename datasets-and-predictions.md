---
description: Faire des itérations et comprendre les prédictions de modèle.
---

# Datasets & Predictions \[Early Access\]

Cette fonctionnalité est actuellement en phase d’accès anticipé. Vous pouvez l’utiliser dans notre service de production sur wandb.ai, avec [quelques limitations](https://docs.wandb.com/untitled#current-limitations). Les APIs sont sujettes à changement. Nous serons ravis d’entendre vos questions, vos commentaires, et vos idées ! Écrivez-nous rapidement à [_feedback@wandb.com_](mailto:feedback@wandb.com)_._

_Les données sont au cœur de tous les flux de travail d’apprentissage automatique. Nous avons ajouté de nouvelles fonctionnalités puissantes aux Artefacts W&B pour vous permettre de visualiser et faire des requêtes de datasets et de d’évaluation de modèle au niveau d’exemple. Vous pouvez utiliser ce nouvel outil pour analyser et comprendre vos datasets, et pour mesurer et déboguer les performances de votre modèle._

_Plongez dans cette expérience avec une démo bout-à-bout_ : [![](https://colab.research.google.com/assets/colab-badge.svg)](http://wandb.me/dsviz-demo-colab)

![Voici un aper&#xE7;u de l&#x2019;IU du tableau de dataset](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673880422_image.png)

## Comment ça fonctionne

 Notre but est de vous donner des outils hautement scalables, flexibles et configurables, avec des visuels riches et prêts à être utilisés disponibles pour les tâches communes. Ce système est construit à partir de :

* La capacité de sauvegarder de grands objets wandb.Table, qui contiennent optionnellement des médias lourds \(comme des images avec boîtes à limite minimum\), à l’intérieur d’Artefacts W&B.
* La prise en charge de références de fichiers inter-artefacts, et la capacité à combiner les tableaux ensemble dans l’IU. C’est utilisé, par exemple, pour enregistrer un set de prédictions de boîtes à limite minimum contre un artefact dataset vérité-terrain, sans dupliquer les images sources et les libellés.
* \[futur\] Prise en charge backend API pour les requêtes à grande échelle sur des tableaux stockés dans Artefacts W&B.
* Une toute nouvelle “architecture saisie de panneau d’IU échangeable en fonction du run-time“. C’est ce qui nourrit les visuels et les graphiques puissants que vous voyez pendant que vous comparez et que vous regroupez vos tableaux de données. Nous finirons par ouvrir ceci, pour que les utilisateurs puissent ajouter des visuels complètement personnalisés qui fonctionnent partout dans l’IU W&B.

## UI

Suivez en ouvrant ce [projet d’exemple](https://wandb.ai/shawn/dsviz_demo/artifacts/dataset/train_results/18bab424be78561de9cd/files), qui a été généré depuis notre [démo colab](http://wandb.me/dsviz-demo-colab).

 Pour visualiser les tableaux et les objets médias enregistrés, ouvrez un artefact, rendez-vous dans l’onglet **Fichiers** \(Files\), et cliquez sur le tableau ou l’objet. Passez en Vue de Graphique \(Graph View\) pour voir comment les artefacts et les essais sont connectés dans ce projet. Activez **Exploser** \(Explode\) pour voir les exécutions individuelles de chaque étape dans le pipeline.

###  ****Visualiser des tableaux

 Ouvrez l’onglet **Fichiers** \(Files\) pour voir l’IU de visualisation principale. Dans ce [projet d’exemple](https://wandb.ai/shawn/dsviz_demo/artifacts/dataset/train_results/18bab424be78561de9cd/files), cliquez sur “train\_iou\_score\_table.table.json” pour le visualiser. Pour explorer les données, vous pouvez filtrer, regrouper et trier le tableau.

![](.gitbook/assets/image%20%2899%29.png)

### Filtrage

Les filtres sont spécifiés dans le [langage d’agrégation mongodb](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/), qui a une bonne prise en charge des requêtes d’objets imbriqués \[note : nous n’utilisons pas vraiment mongodb sur notre backend !\]. Voici deux exemples :

 **Trouver des exemples qui ont &gt;0.05 dans la colonne “road”** 

`{$gt: ['$0.road', 0.05]}` 

 **Trouver des exemples qui ont une ou plus prédictions de boîte à limite minimum “car”**

`{  
  $gte: [   
    {$size:   
      {$filter:   
        {input: '$0.pred_mask.boxes.ground_truth',  
          as: 'box',  
          cond: {$eq: ['$$box.class_id', 13]}  
        }  
      }  
    },  
  1]  
}`

### Regroupement

 Essayez de regrouper par “dominant\_pred”. Vous verrez que les colonnes numériques deviennent automatiquement des histogrammes lorsque le tableau est groupé.

![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673736462_image.png)

\*\*\*\*

###  ****Tri

Cliquez sur **Trier** \(Sort\) et choisissez n’importe quelle colonne du tableau pour le trier en fonction de cette colonne.

### Comparaison

 Comparez n’importe quelles deux versions d’artefact dans le tableau. Passez la souris sur “v3” dans la barre latérale, et cliquez sur le bouton “Compare”. Cela vous montrera les prédictions des deux versions en un seul tableau. Dites-vous que les deux tableaux sont placés l’un sur l’autre. Le tableau décide de tracer des graphiques en arbre pour les colonnes numériques entrantes, avec une barre pour chaque tableau qui est comparé.

Vous pouvez utiliser les “Filtres rapides” \(Quick filters\) tout en haut pour limiter vos résultats à des exemples qui ne sont présents que dans les deux versions.

![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673764298_image.png)

  
 Essayez de comparer et de regrouper à la fois. Vous obtiendrez un “multi-histogramme”, où nous utilisons une couleur par tableau entrant.  


![](https://paper-attachments.dropbox.com/s_21D0DE4B22EAFE9CB1C9010CBEF8839898F3CCD92B5C6F38DBE168C2DB868730_1605673664913_image.png)

## API Python

Essayez notre [démo colab](http://wandb.me/dsviz-demo-colab) pour avoir un exemple de bout-en-bout.

Pour visualiser des datasets et des prédictions, enregistrez des médias lourds dans un artefact. En plus de sauvegarder vos fichiers bruts dans les Artefacts W&B, vous pouvez maintenant sauvegarder, reprendre, et visualiser d’autres types de médias lourds fournis par l’API wandb.

 Les types suivants sont actuellement pris en charge :

* wandb.Table\(\)
* wandb.Image\(\)

 La prise en charge de types supplémentaires de médias est prévue pour bientôt.

### Nouvelles méthodes artefacts

 Il y a deux nouvelles méthodes pour les objets Artifact :

`artifact.add(object, name)`

* Ajoute un objet média à un artefact. Pour l’instant, les types pris en charge sont wandb.Table et wandb.Image, et d’autres arriveront bientôt.
* Récursivement, ajoute tout objet média et ressource child \(comme les fichiers  ‘.png’ bruts\) à cet artefact.

`artifact.get(name)`

* Renvoie un objet média reconstitué depuis un artefact stocké.

Ces méthodes sont symétriques. Vous pouvez stocker un objet dans un artefact en utilisant .add\(\), et être sûr que vous obtiendrez exactement le même objet en utilisant .get\(\), sur n’importe quelle machine qui en a besoin.

###  ****Objets médias wandb.\*

`wandb.Table`

Les tableaux sont au cœur des visualisations de dataset et de de prédictions. Pour visualiser un dataset, placez un wandb.Table, ajoutez des objets wandb.Image, des arrays, des dictionnaires, des chaînes de données et de nombres à votre goût, puis ajoutez votre tableau à un artefact. Actuellement, chaque tableau est limité à 50 000 lignes. Vous pouvez enregistrer autant de tableaux que vous le souhaitez dans un artefact.

L’exemple de code qui suit sauvegarde 1 000 images et libellés depuis le dataset Keras cifar10 test en tant que wandb.Table dans un artefact : 

```python
import tensorflow as tf
import wandb

classes = ['airplane', 'automobile', 'bird', 'cat',
           'deer', 'dog', 'frog', 'horse', 'ship', 'truck']
_, (x_test, y_test) = tf.keras.datasets.cifar10.load_data()

wandb.init(job_type='create-dataset') # start tracking program execution

# construct a table containing our dataset
table = wandb.Table(('image', 'label'))
for x, y in zip(x_test[:1000], y_test[:1000]):
    table.add_data(wandb.Image(x), classes[y[0]])

# put the table in an artifact and save it
dataset_artifact = wandb.Artifact('my-dataset', type='dataset')
dataset_artifact.add(table, 'dataset')
wandb.log_artifact(dataset_artifact)
```

Après avoir exécuté ce code, vous pourrez visualiser le tableau dans l’IU W&B. Cliquez sur “dataset.table.json” dans l’onglet Fichiers d’artefact. Essayez de regrouper par “label” pour avoir des exemples de chaque classe dans la colonne “image”.

  
`wandb.Image`

Vous pouvez construire des objets wandb.Image, comme décrit dans notre [documentation wandb.log](https://docs.wandb.com/library/log#images-and-overlays).

wandb.Image vous permet d’attacher des masques de segmentation et des boîtes à limite minimum à des images, comme spécifié dans les documents au-dessus. Lorsque vous sauvegardez des objets wandb.Image\(\) dans des artefacts, il y a un changement : nous avons neutralisé  “class\_labels”, qui avait précédemment besoin d’être stocké dans chaque wandb.Image.

Maintenant, vous devriez créer des libellés de classes séparément, si vous utilisez des boîtes à limite minimum ou des masques de segmentation, comme ceci :

```python
class_set = wandb.Classes(...)
example = wandb.Image(<path_to_image_file>, classes=class_set, masks={
            "ground_truth": {"path": <path_to_mask>}})
```

 Vous pouvez aussi construire une wandb.Image qui fait référence à une wandb.Image qui a été enregistrée dans un artefact différent. Cela utilisera la référence-fichier-inter-artefact \(cross-artifact-file-reference\) pour éviter de dupliquer l’image sous-jacente.

```python
artifact = wandb.use_artifact('my-dataset-1:v1')
dataset_image = artifact.get('an-image')  # if you've logged a wandb.Image here 
predicted_image = wandb.Image(dataset_image, classes=class_set, masks={
            "predictions": {"path": <path_to_mask>}})
```

  
`wandb.Classes`

Utilisé pour définir un mapping depuis un id de classe \(un nombre\) vers un libellé \(une chaîne\) :

```python
CLASSES = ['dog', 'cat']
class_set = wandb.Classes([{'name': c, 'id': i} for i, c in enumerate(CLASSES)])
```



`wandb.JoinedTable`

Utilisé pour dire à l’IU W&B de faire le rendering de la combinaison de deux tableaux. Les tableaux peuvent être stockés dans d’autres artefacts.

```python
jt = wandb.JoinedTable(table1, table2, 'id')
artifact.add(jt, 'joined')
```

##  Exemples bout-en-bout

 Essayez notre [notebook colab](http://wandb.me/dsviz-demo-colab) pour voir un exemple qui couvre de bout-en-bout :

* dla construction et la visualisation de dataset
* l’entraînement de modèle
*  l’enregistrement de prédictions contre un dataset et leur visualisation

## FAQ

**J’emballe mon dataset dans un format binaire pour l’entraînement. Comment cela fonctionne avec les Artefacts de Dataset W&B ?**

Vous pouvez prendre plusieurs approches, ici :

1. Utiliser le format wandb.Table comme système d’enregistrement pour vos datasets. De-là, vous avez deux possibilités :
   1. au moment de l’entraînement, dérivez un format emballé depuis l’artefact de format W&B
   2. OU, incluez une étape pipeline qui produit un artefact de format emballé, et entraînez depuis cet artefact
2. Stockez votre format emballé et le wandb.Table dans le même artefact
3. Créez une tâche qui, étant donné votre format emballé, enregistre un artefact wandb.Table

\[note : dans la phase Alpha, il y a une limite de 50 000 lignes par tableau sauvegardé dans les Artefacts W&B\] Si vous voulez faire des requêtes et visualiser les prédictions de votre modèle, vous devez prendre en compte comment passer les ID d’exemples à travers vos étapes d’entraînement, pour que votre tableau de prédiction puisse être combiné de nouveau avec le tableau source de dataset. Consultez les exemples dont nous avons mis les liens pour voir différentes approches. Plus tard, nous fournirons des convertisseurs pour les formats communs, de nombreux autres exemples, et des intégrations approfondies pour les frameworks populaires.

## Limites actuelles

Cette fonctionnalité est actuellement en phase d’accès anticipé. Vous pouvez l’utiliser dans notre service de production sur wandb.ai, avec quelques limitations. Les APIs sont sujettes à changement. Si vous avez des questions, des commentaires, ou des idées, nous voulons vous parler ! Écrivez-nous rapidement à [_feedback@wandb.com_](mailto:feedback@wandb.com)_._

* _Échelle : les tableaux enregistrés dans Artefacts sont actuellement limités à 50 000 lignes. Nous augmenterons ce nombre à chaque nouvelle mise à jour, avec comme but la prise en charge de plus de 100 millions de lignes par tableaux à l’avenir._
*  _Types de médias wandb.\* actuellement pris en charge :_
  * wandb.Table
  * wandb.Image
*  _Il n’y a aucun moyen de sauvegarder ou de faire persister des requêtes et des vues dans l’IU W&B._
*    _****Il n’y a aucun moyen d’ajouter des visuels pour les Rapports W&B._

## _Travail en cours_

* Tout un tas d’améliorations UX & UI
* Augmentation de limite de lignes par tableau
* Utilisation d’un format binaire de colonnes \(parquet\) pour le stockage de tableau
* Prise en charge des dataframes et d’autres formats de tableaux communs python, en plus de wandb.Table
* Ajout d’un système de requête plus puissant, qui prend en charge l’agrégation et l’analyse plus approfondies.
* Prise en charge de plus de types de médias
* Ajout de la capacité à faire persister l’état de l’IU via la sauvegarde de vues/de workspace
* Capacité à sauvegarder les visuels & analyses dans les Rapports W&B, pour partager avec des collègues
* Capacité à sauvegarder des requêtes et des sous-sets pour relibeller et pour d’autres flux de travaux
* Capacité à faire une requête depuis Python
* Capacité à ajouter d’autres visuels qui utilisent des données de grands tableaux \(comme un visualizer de cluster\)
* Prise en charge de panneaux dont l’utilisateur est l’auteur, pour des visuels complètement personnalisés

