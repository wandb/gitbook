---
description: >-
  Garder une trace de vos mesures, de vos vidéos, de vos graphiques linéaires
  personnalisés, et plus
---

# wandb.log\(\)

Appelez `wandb.log(dict)`pour enregistrer un dictionnaire de vos mesures ou d’objets personnalisés par étape \(step\). Chaque fois que vous enregistrez, nous incrémentons l’étape par défaut, ce qui vous permet de voir vos mesures au fil du temps.

###  Exemple d’utilisation

```python
wandb.log({'accuracy': 0.9, 'epoch': 5})
```

###  ****Flux de travaux courant

1. **Comparez la meilleure précision :** Pour comparer la meilleure valeur d’une mesure sur plusieurs essais, réglez la valeur du sommaire sur cette mesure. Par défaut, le sommaire est réglé sur la dernière valeur que vous avez enregistré pour chaque clef. C’est utile sur le tableau dans l’IU, où vous pouvez classifier et filtrer vos essais en se basant sur leurs mesures de sommaire – pour que vous puissiez comparer vos essais dans un tableau ou un diagramme à barres en se basant sur leur meilleure précision, plutôt que sur la précision finale. Par exemple, vous pouvez régler le sommaire \(summary\) comme ceci :`wandb.run.summary["accuracy"] = best_accuracy`
2. **De multiples mesures en un seul graphique** : Enregistrez plusieurs mesures avec le même appel dans wandb.log\(\), comme ceci :    ****`wandb.log({'acc': 0.9, 'loss': 0.1})` ****et elles seront toutes les deux mises à disposition pour les comparer à…
3. **L’axe x personnalisé** : Ajoutez un axe x personnalisé au même appel d’enregistrement pour visualiser vos mesures sur un axe différent depuis le Tableau de bord W&B. Par exemple : `wandb.log({'acc': 0.9, 'custom_step': 3})`

###  Documentation de référence

 Consultez la docu de référence, générée à partir de la librairie Python `wandb`

{% page-ref page="../ref/run.md" %}

## Enregistrer des objets

Nous prenons en charge les images, les vidéos, l’audio, les graphiques personnalisés, et plus encore. Enregistrez des médias lourds pour explorer vos résultats et visualisez les comparaisons entre vos essais.

[ ](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK) [Testez-le dans Colab](https://colab.research.google.com/drive/15MJ9nLDIXRvy_lCwAou6C2XN3nppIeEK)

###  Histogrammes

```python
wandb.log({"gradients": wandb.Histogram(numpy_array_or_sequence)})
wandb.run.summary.update({"gradients": wandb.Histogram(np_histogram=np.histogram(data))})
```

Si une séquence est fournie dans le premier argument, nous créerons les colonnes de l’histogramme automatiquement. Vous pouvez aussi coller ce qui est rendu de np.histogram à l’argument clef np\_histogram pour créer vos propres regroupements. Le nombre maximal de regroupement supporté est de 512. Vous pouvez utiliser l’argument clef optionnel num\_bins quand vous lancez une séquence pour ignorer les 64 regroupements possibles par défaut.

Si les histogrammes sont dans votre sommaire, ils apparaîtront sous forme de sparklines sur les pages individuelles de run. S’ils sont dans votre historique, nous créons une heatmap de colonnes au fur et à mesure.

###  Images et Overlays

{% tabs %}
{% tab title="Image" %}
`wandb.log({"examples": [wandb.Image(numpy_array_or_pil, caption="Label")]})`

 Si une numpy array est fournie, nous supposons que c’est un niveau de gris si la dernière dimension est à 1, un RGB si elle est à 3, et un RGBA si elle est à 4. Si l’array contient des floats, nous les convertissons en ints comprises entre 0 et 255. Vous pouvez manuellement spécifier un [mode](https://pillow.readthedocs.io/en/3.1.x/handbook/concepts.html#concept-modes) ou simplement fournir une `PIL.Image` . Il est recommandé d’enregistrer moins de 50 images par étape.
{% endtab %}

{% tab title="Masque de Segmentation :" %}
Si vous avez des images avec des masques pour une segmentation sémantique, vous pouvez enregistrer les masques et les activer ou les désactiver dans l’IU. Pour enregistrer plusieurs masques, enregistrez un dictionnaire de masque avec plusieurs clefs. Voici un exemple :

* **mask\_data**: une numpy array 2D qui contient une étiquette de classe de nombre entier pour chaque pixel
* **class\_labels**: un dictionnaire qui attribue les nombres de **mask\_data** à des étiquettes lisibles

```python
mask_data = np.array([[1, 2, 2, ... , 2, 2, 1], ...])

class_labels = {
  1: "tree",
  2: "car",
  3: "road"
}

mask_img = wandb.Image(image, masks={
  "predictions": {
    "mask_data": mask_data,
    "class_labels": class_labels
  },
  "groud_truth": {
    ...
  },
  ...
})
```

 [Voir un exemple en direct →](https://app.wandb.ai/stacey/deep-drive/reports/Image-Masks-for-Semantic-Segmentation--Vmlldzo4MTUwMw)

 [Échantillon de code →](https://colab.research.google.com/drive/1SOVl3EvW82Q4QKJXX6JtHye4wFix_P4J)

![](../.gitbook/assets/semantic-segmentation.gif)
{% endtab %}

{% tab title="Bounding Box" %}
Log bounding boxes with images, and use filters and toggles to dynamically visualize different sets of boxes in the UI.

```python
class_id_to_label = {
    1: "car",
    2: "road",
    3: "building",
    ....
}

img = wandb.Image(image, boxes={
    "predictions": {
        "box_data": [{
            "position": {
                "minX": 0.1,
                "maxX": 0.2,
                "minY": 0.3,
                "maxY": 0.4,
            },
            "class_id" : 2,
            "box_caption": "minMax(pixel)",
            "scores" : {
                "acc": 0.1,
                "loss": 1.2
            },
        }, 
        # Log as many boxes as needed
        ...
        ],
        "class_labels": class_id_to_label
    },
    "ground_truth": {
    # Log each group of boxes with a unique key name
    ...
    }
})

wandb.log({"driving_scene": img})
```

Optional Parameters

`class_labels` An optional argument mapping your class\_ids to string values. By default we will generate class\_labels `class_0`, `class_1`, etc...

Boxes - Each box passed into box\_data can be defined with different coordinate systems.

`position`

* Option 1: `{minX, maxX, minY, maxY}` Provide a set of coordinates defining the upper and lower bounds of each box dimension.
* Option 2: `{middle, width, height}`  Provide a set of coordinates specifying the middle coordinates as `[x,y]`, and `width`, and `height` as scalars 

`domain` Change the domain of your position values based on your data representation

* `percentage` \(Default\) A relative value representing the percent of the image as distance
* `pixel`An absolute pixel value

[See a live example →](https://app.wandb.ai/stacey/yolo-drive/reports/Bounding-Boxes-for-Object-Detection--Vmlldzo4Nzg4MQ)

![](../.gitbook/assets/bb-docs.jpeg)
{% endtab %}
{% endtabs %}

### Médias

{% tabs %}
{% tab title="Audio" %}
```python
wandb.log({"examples": [wandb.Audio(numpy_array, caption="Nice", sample_rate=32)]})
```

Le nombre maximal de clip audio qui peut être enregistré par étape est de 100.
{% endtab %}

{% tab title="Vidéo :" %}
```python
wandb.log({"video": wandb.Video(numpy_array_or_path_to_video, fps=4, format="gif")})
```

 Si une numpy array est fournie, nous supposons que les dimensions sont : temps \(time\), canaux \(channels\), largeur \(width\), hauteur \(height\). Par défaut, nous créons une image gif à 4 ips \(ffmepg et la librairie python moviepy sont nécessaire lorsque vous placez des objets numpy\). Les formats pris en charge sont « gif », « mp4 », « webm », et « ogg ». Si vous ajoutez une chaîne à `wandb.Video`, nous vérifions bien que le fichier existe et que son format est pris en charge avant de le télécharger sur wandb. Ajouter un objet BytesIO créera un fichier temporaire tempfile avec le format spécifié dans son extension.

 Sur la page de runs W&B, vous pouvez voir vos vidéos sous la section Médias.
{% endtab %}

{% tab title="Tableau de texte :" %}
Utilisez wandb.Table\(\) pour enregistrer du texte dans des tableaux qui s’affichera dans l’IU. Par défaut, les titres des colonnes sont `["Input", "Output", "Expected"]`. Le nombre maximal de lignes est de 10 000.

```python
# Method 1
data = [["I love my phone", "1", "1"],["My phone sucks", "0", "-1"]]
wandb.log({"examples": wandb.Table(data=data, columns=["Text", "Predicted Label", "True Label"])})

# Method 2
table = wandb.Table(columns=["Text", "Predicted Label", "True Label"])
table.add_data("I love my phone", "1", "1")
table.add_data("My phone sucks", "0", "-1")
wandb.log({"examples": table})
```

 Vous pouvez aussi passer un objet pandas `DataFrame` .

```python
table = wandb.Table(dataframe=my_dataframe)
```
{% endtab %}

{% tab title="HTML" %}
```python
wandb.log({"custom_file": wandb.Html(open("some.html"))})
wandb.log({"custom_string": wandb.Html('<a href="https://mysite">Link</a>')})
```

Un html personnalisé peut être enregistré à n’importe quelle clef, ce qui affiche un panneau HTML sur la page run. Par défaut, nous injectons les styles de base, vous pouvez les désactiver en ajoutant `inject=False` .

```python
wandb.log({"custom_file": wandb.Html(open("some.html"), inject=False)})
```
{% endtab %}

{% tab title="Molecule" %}
```python
wandb.log({"protein": wandb.Molecule(open("6lu7.pdb"))}
```

Enregistrez des données moléculaires sous 10 types de fichiers différents.

`'pdb', 'pqr', 'mmcif', 'mcif', 'cif', 'sdf', 'sd', 'gro', 'mol2', 'mmtf'`

Lorsque votre essai sera fini, vous pourrez interagir avec des visuels 3D de vos molécules dans l’IU.

 [Voir un exemple en direct →](https://app.wandb.ai/nbaryd/Corona-Virus/reports/Visualizing-Molecular-Structure-with-Weights-%26-Biases--Vmlldzo2ODA0Mw)

![](../.gitbook/assets/docs-molecule.png)
{% endtab %}
{% endtabs %}

###  Graphiques personnalisés

Ces préréglages ont des méthodes `wandb.plot` intégrées pour rendre l’enregistrement de graphiques rapide, directement depuis votre script, et pour voir les visualisations exactes que vous recherchez dans l’IU.

{% tabs %}
{% tab title="Linéaire " %}
`wandb.plot.line()`

 Enregistrez un graphique linéaire personnalisé – une liste de points ordonnés et connectés \(x,y\) sur des axes x et y arbitraires.

```python
data = [[x, y] for (x, y) in zip(x_values, y_values)]
table = wandb.Table(data=data, columns = ["x", "y"])
wandb.log({"my_custom_plot_id" : wandb.plot.line(table, "x", "y", title="Custom Y vs X Line Plot")})
```

Vous pouvez utiliser cet outil pour enregistrer des courbes dans n’importe quelles deux dimensions. Notez bien que si vous tracez deux listes de valeurs l’une contre l’autre, le nombre de valeurs de ces listes doit correspondre exactement \(i.e. chaque point doit avoir un x et un y\).

![](../.gitbook/assets/line-plot.png)

[Voir dans l’app →](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

 [Exécuter le code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Dispersion " %}
`wandb.plot.scatter()`

Enregistrez un diagramme de dispersion personnalisé – une liste de points \(x,y\) sur une paire d’axes x et y arbitraires.

```python
data = [[x, y] for (x, y) in zip(class_x_prediction_scores, class_y_prediction_scores)]
table = wandb.Table(data=data, columns = ["class_x", "class_y"])
wandb.log({"my_custom_id" : wandb.plot.scatter(table, "class_x", "class_y")})
```

Vous pouvez utiliser cet outil pour disperser des points dans n’importe quelles deux dimensions. Notez bien que si vous tracez deux listes de valeurs l’une contre l’autre, le nombre de valeurs de ces listes doit correspondre exactement \(i.e. chaque point doit avoir un x et un y\).

![](../.gitbook/assets/demo-scatter-plot.png)

 [Voir dans l’app →](https://wandb.ai/wandb/plots/reports/Custom-Scatter-Plots--VmlldzoyNjk5NDQ)

 [Exécuter le code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="En barres " %}
`wandb.plot.bar()`

Enregistrez un diagramme en barres personnalisé – une liste de valeurs étiquetées sous forme de barres – nativement en quelques lignes :

```python
data = [[label, val] for (label, val) in zip(labels, values)]
table = wandb.Table(data=data, columns = ["label", "value"])
wandb.log({"my_bar_chart_id" : wandb.plot.bar(table, "label", "value", title="Custom Bar Chart")
```

Vous pouvez utiliser cet outil pour enregistrer des diagrammes en barres arbitraires. Notez bien que le nombre d’étiquettes et de valeurs dans ces listes doit exactement correspondre. \(i.e. chaque point de donnée doit avoir les deux\).

![](../.gitbook/assets/image%20%2896%29.png)

 [Voir dans l’app →](https://wandb.ai/wandb/plots/reports/Custom-Bar-Charts--VmlldzoyNzExNzk)

 [Exécuter le code →](https://colab.research.google.com/drive/1uXLKDmsYg7QMRVFyjUAlg-eZH2MW8yWH?usp=sharing)
{% endtab %}

{% tab title="Histogramme " %}
`wandb.plot.histogram()`

Enregistrez un histogramme personnalisé – classez une liste de valeurs en lots par décompte/fréquence d’occurrence – nativement, en quelques lignes. Admettons que j’ai une liste de scores de prédiction de confiance \( scores \) et que je veux visualiser leur distribution.

```python
data = [[s] for s in scores]
table = wandb.Table(data=data, columns=["scores"])
wandb.log({'my_histogram': wandb.plot.histogram(table, "scores", title=None)})
```

Vous pouvez utiliser cet outil pour enregistrer des histogrammes arbitraires. Notez bien que `data` est une liste de listes, dont le but est d’accueillir un ensemble 2D de lignes et de colonnes.

![](../.gitbook/assets/demo-custom-chart-histogram.png)

[Voir dans l’app →](https://wandb.ai/wandb/plots/reports/Custom-Histograms--VmlldzoyNzE0NzM) 

 [Exécuter le code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="PR" %}
`wandb.plot.pr_curve()`

Enregistrez une [courbe Précision-Rappel](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_recall_curve.html#sklearn.metrics.precision_recall_curve) en une seule ligne :

```python
wandb.log({"pr" : wandb.plot.pr_curve(ground_truth, predictions,
                     labels=None, classes_to_plot=None)})
```

Vous pouvez enregistrer ceci du moment que votre code a accès aux éléments suivants :

* les scores prédits d’un modèle \( `predictions` \) sur un ensemble d’exemples
* les étiquettes vérité-terrain correspondantes \( `ground_truth` \) de ces exemples
* \(optionnel\) une liste des noms d’étiquettes/de classe \( `labels=["chat", "chien", "oiseau"...`\] si l’index d’étiquette 0 veut dire chat, 1 = chien, 2 = oiseau, etc.\)
* \(optionnel\) un sous-ensemble \(toujours en format liste\) de ces étiquettes à visualiser dans le graphique

![](../.gitbook/assets/demo-precision-recall.png)

 [Voir dans l’app →](https://wandb.ai/wandb/plots/reports/Plot-Precision-Recall-Curves--VmlldzoyNjk1ODY)

 [Exécuter le code →](https://colab.research.google.com/drive/1mS8ogA3LcZWOXchfJoMrboW3opY1A8BY?usp=sharing)
{% endtab %}

{% tab title="Courbe ROC" %}
`wandb.plot.roc_curve()`

 Enregistrez une [courbe ROC](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html#sklearn.metrics.roc_curve) en une seule ligne :

```text
wandb.log({"roc" : wandb.plot.roc_curve( ground_truth, predictions, \
                        labels=None, classes_to_plot=None)})
```

Vous pouvez enregistrer ceci du moment que votre code a accès aux éléments suivants :

*  les scores prédits d’un modèle \( `predictions` \) sur un ensemble d’exemples
*  es étiquettes vérité-terrain correspondantes \( `ground_truth` \) de ces exemples
* \(optionnel\) une liste des noms d’étiquettes/de classe \( `labels=["chat", "chien", "oiseau"...`\] si l’index d’étiquette 0 veut dire chat, 1 = chien, 2 = oiseau, etc.\)
* \(optionnel\) un sous-ensemble \(toujours en format liste\) de ces étiquettes à visualiser dans le graphique

![](../.gitbook/assets/demo-custom-chart-roc-curve.png)

[Voir dans l’app →](https://wandb.ai/wandb/plots/reports/Plot-ROC-Curves--VmlldzoyNjk3MDE)

[Exécuter le code →](https://colab.research.google.com/drive/1_RMppCqsA8XInV_jhJz32NCZG6Z5t1RO?usp=sharing)
{% endtab %}

{% tab title="Matrice de confusion" %}
`wandb.plot.confusion_matrix()`

 Enregistrez une [matrice de confusion](https://scikit-learn.org/stable/auto_examples/model_selection/plot_confusion_matrix.html) multi-classe en une seule ligne :

```text
wandb.log({"conf_mat" : wandb.plot.confusion_matrix(
                        predictions, ground_truth, class_names})
```

Vous pouvez enregistrer ceci du moment que votre code a accès aux éléments suivants :

* les scores prédits d’un modèle \( `predictions` \) sur un ensemble d’exemples
* les étiquettes vérité-terrain correspondantes \( `ground_truth` \) de ces exemples
* une liste complète des noms d’étiquettes/de classe sous forme de chaînes de données \( `labels=["chat", "chien", "oiseau"...`\] si l’index d’étiquette 0 veut dire chat, 1 = chien, 2 = oiseau, etc.\)

![](../.gitbook/assets/image%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29%20%281%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%281%29.png)

​​ [Voir dans l’app →](https://wandb.ai/wandb/plots/reports/Confusion-Matrix--VmlldzozMDg1NTM)

​ [Exécuter le code →](https://colab.research.google.com/drive/1OlTbdxghWdmyw7QPtSiWFgLdtTi03O8f?usp=sharing)
{% endtab %}
{% endtabs %}





###  ****Préréglages personnalisés

 Ajustez un préréglage déjà présent de Graphique Personnalisé ou créez un nouveau préréglage, puis sauvegardez le graphique. Utilisez l’ID du graphique pour enregistrer vos données dans ce préréglage personnalisé directement depuis votre script.

```python
# Create a table with the columns to plot
table = wandb.Table(data=data, columns=["step", "height"])

# Map from the table's columns to the chart's fields
fields = {"x": "step",
          "value": "height"}

# Use the table to populate the new custom chart preset
# To use your own saved chart preset, change the vega_spec_name
my_custom_chart = wandb.plot_table(vega_spec_name="carey/new_chart",
              data_table=table,
              fields=fields,
              )
```

 [Exécuter le code →](https://tiny.cc/custom-charts)

### Matplotlib

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.ylabel('some interesting numbers')
wandb.log({"chart": plt})
```

 Vous pouvez inscrire un pyplot `matplotlib` ou un figure object dans `wandb.log()` . Par défaut, nous convertissons le plot en un plot [Plotly](https://plot.ly/). Si vous voulez explicitement enregistrer ce plot sous forme d’image, passer ce plot dans `wandb.Image` . Nous acceptons aussi l’enregistrement direct de graphiques Plotly.

### Visualisations 3D

{% tabs %}
{% tab title="Objet 3D" %}
 Enregistrez des fichiers aux formats `obj`, `gltf`, ou `glb,` et nous vous en donnerons un rendering dans l’IU lorsque votre essai se finira.

```python
wandb.log({"generated_samples":
           [wandb.Object3D(open("sample.obj")),
            wandb.Object3D(open("sample.gltf")),
            wandb.Object3D(open("sample.glb"))]})
```

![V&#xE9;rit&#xE9;-terrain et pr&#xE9;diction d&#x2019;un nuage de points d&#x2019;un casque de musique](../.gitbook/assets/ground-truth-prediction-of-3d-point-clouds.png)

[Voir un exemple en direct →](https://app.wandb.ai/nbaryd/SparseConvNet-examples_3d_segmentation/reports/Point-Clouds--Vmlldzo4ODcyMA)
{% endtab %}

{% tab title="Nuages de points" %}
Enregistrez des nuages de points 3D et des scènes Lidar avec des boîtes à limite minimum. Inscrivez une numpy array qui contient des coordonnées et des couleurs pour que les points s’affichent.

```python
point_cloud = np.array([[0, 0, 0, COLOR...], ...])

wandb.log({"point_cloud": wandb.Object3D(point_cloud)})
```

 Trois formes différentes de numpy arrays sont acceptées pour avoir des schémas de couleur flexibles.

* `[[x, y, z], ...]` `nx3`
* `[[x, y, z, c], ...]` `nx4` `| c is a category` c est une catégorie\) dans l’ensemble `[1, 14]` \(Utile pour de la segmentation\)
* `[[x, y, z, r, g, b], ...]` `nx6 | r,g,b` sont des valeurs dans l’ensemble `[0,255]`pour les canaux de couleurs rouge, vert et bleu.

Voici un exemple de code d’enregistrement ci-dessous :

* `points`est une numpy array avec le même format que le nuage de points simple montré à la section précédente.
* `boxes` est une numpy array de dictionnaires python avec trois attributs :
  * `corners`- une liste de huit coins
  * `label`- une ligne représentant l’étiquette qui devra être affichée dans la boîte \(Optionnel\)
  * `color`- les valeurs rgb qui représentent la couleur de la boîte
* `type` est une chaîne de données \(string\) qui représente le type de scène à afficher. Pour l’instant, la seule valeur prise en charge est`lidar/beta`

```python
# Log points and boxes in W&B
wandb.log(
        {
            "point_scene": wandb.Object3D(
                {
                    "type": "lidar/beta",
                    "points": np.array(
                        [
                            [0.4, 1, 1.3], 
                            [1, 1, 1], 
                            [1.2, 1, 1.2]
                        ]
                    ),
                    "boxes": np.array(
                        [
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,1,0],
                                    [0,0,1],
                                    [1,0,0],
                                    [1,1,0],
                                    [0,1,1],
                                    [1,0,1],
                                    [1,1,1]
                                ],
                                "label": "Box",
                                "color": [123,321,111],
                            },
                            {
                                "corners": [
                                    [0,0,0],
                                    [0,2,0],
                                    [0,0,2],
                                    [2,0,0],
                                    [2,2,0],
                                    [0,2,2],
                                    [2,0,2],
                                    [2,2,2]
                                ],
                                "label": "Box-2",
                                "color": [111,321,0],
                            }
                        ]
                    ),
                    "vectors": [
                        {"start": [0,0,0], "end": [0.1,0.2,0.5]}
                    ]
                }
            )
        }
    )
```
{% endtab %}
{% endtabs %}

## Enregistrement par incréments

Si vous souhaitez visualiser vos mesures sur différents axes X, vous pouvez enregistrer l’étape comme une mesure, comme ceci `wandb.log({'loss': 0.1, 'epoch': 1, 'batch': 3})` . Dans l’IU, vous pouvez naviguer entre les axes X dans les paramètres de graphique.

 Si vous souhaitez enregistrer une seule étape d’historique depuis beaucoup d’endroits différents dans votre code, vous pouvez ajouter un index d’étape à `wandb.log()` comme suit :

```python
wandb.log({'loss': 0.2}, step=step)
```

 Tant que vous continuez à renseigner la même valeur pour `step (étape)`, W&B rassemblera les clefs et les valeurs de chaque appel en un seul dictionnaire unifié. Dès que vous appelez `wandb.log()` avec une valeur différente de la précédente pour `step` , W&B écrira toutes les clefs et les valeurs récoltées dans l’historique, et recommencera son processus de rassemblement. Notez bien que cela signifie que vous ne devez utiliser ceci que pour des valeurs consécutives de `step` : 0, 1, 2, …. Cette fonctionnalité ne vous laisse pas écrire à n’importe quelle étape de l’historique que vous pourriez souhaitez, simplement l’étape « en cours » et l’étape « suivante ».

 Vous pouvez aussi régler **commit=False** dans `wandb.log` pour accumuler des mesures, mais assurez-vous d’appeler `wandb.log` sans le flag **commit** pour persister dans vos mesures.

```python
wandb.log({'loss': 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
wandb.log({'accuracy': 0.8})
```

##  Mesures de sommaire

 Les statistiques de sommaire sont utilisées pour garder une trace des mesures isolées par modèle. Si une mesure de sommaire est modifiée, seul l’état mis à jour est sauvegardé. Nous réglons automatiquement le sommaire sur la dernière ligne de l’historique, à moins que ce ne soit modifié manuellement. Si vous modifiez une mesure du sommaire, nous ne faisons persister que la dernière valeur sur laquelle elle a été réglée.

```python
wandb.init(config=args)

best_accuracy = 0
for epoch in range(1, args.epochs + 1):
  test_loss, test_accuracy = test()
  if (test_accuracy > best_accuracy):
    wandb.run.summary["best_accuracy"] = test_accuracy
    best_accuracy = test_accuracy
```

Vous pouvez avoir envie de stocker vos mesures d’évaluation dans un sommaire de vos runs après la fin de vos entraînements. Le sommaire peut gérer des numpy arrays, des tenseurs pytorch ou des tenseurs tensorflow. Lorsqu’une valeur est d’un de ces types, nous inscrivons le tenseur entier dans un fichier binaire et nous stockons des mesures de haut niveau dans l’objet sommaire, telles que minimum \(min\), moyenne \(mean\), variance, percentile 95%, etc.

```python
api = wandb.Api()
run = api.run("username/project/run_id")
run.summary["tensor"] = np.random.random(1000)
run.summary.update()
```

### Accéder directement aux enregistrements

L’objet historique est utilisé pour garder une trace des mesures enregistrées par wandb.log. Vous pouvez accéder à un dictionnaire variable des mesures via `run.history.row` . Cette ligne \(row\) sera sauvegardée et une nouvelle ligne sera créée lorsque `run.history.add` ou `wandb.log` seront appelés.

#### Exemple Tensorflow

```python
wandb.init(config=flags.FLAGS)

# Start tensorflow training
with tf.Session() as sess:
  sess.run(init)

  for step in range(1, run.config.num_steps+1):
      batch_x, batch_y = mnist.train.next_batch(run.config.batch_size)
      # Run optimization op (backprop)
      sess.run(train_op, feed_dict={X: batch_x, Y: batch_y})
      # Calculate batch loss and accuracy
      loss, acc = sess.run([loss_op, accuracy], feed_dict={X: batch_x, Y: batch_y})

      wandb.log({'acc': acc, 'loss':loss}) # log accuracy and loss
```

####  Exemple Pytorch

```python
# Start pytorch training
wandb.init(config=args)

for epoch in range(1, args.epochs + 1):
  train_loss = train(epoch)
  test_loss, test_accuracy = test()

  torch.save(model.state_dict(), 'model')

  wandb.log({"loss": train_loss, "val_loss": test_loss})
```

## Questions fréquentes

#### Comparer des images d’epochs différentes

Chaque fois que vous enregistrez des images depuis une étape, nous les sauvegardons pour vous les montrer dans l’IU. Épinglez le panneau image, et utilisez le **navigateur d’étapes** pour observer les images à différentes étapes. Cet outil permet de facilement comparer comment les sorties d’un modèle changent au fur et à mesure de son entraînement.

```python
wandb.log({'epoch': epoch, 'val_acc': 0.94})
```

###  Enregistrement de lots \(batch\)

Si vous souhaitez enregistrer certaines mesures dans chaque lot et avoir des graphiques linéaires standardisés, vous pouvez enregistrer des valeurs d’axes x sur lesquels vous souhaitez tracer vos mesures. Pour ce faire, dans les graphiques linéaires personnalisés, cliquez sur éditer et sélectionner l’axe-x personnalisé \(custom x-axis\).

```python
wandb.log({'batch': 1, 'loss': 0.3})
```

### Enregistrer un PNG

wandb.Image convertit les numpy arrays ou des instances de PILImage en PNG par défaut.

```python
wandb.log({"example": wandb.Image(...)})
# Or multiple images
wandb.log({"example": [wandb.Image(...) for img in images]})
```

###  ****Enregistrer un JPEG

Pour enregistrer un JPEG, vous pouvez indiquer un chemin vers un fichier :

```python
im = PIL.fromarray(...)
rgb_im = im.convert('RGB')
rgb_im.save('myimage.jpg')
wandb.log({"example": wandb.Image("myimage.jpg")})
```

###  ****Enregistrer une vidéo

```python
wandb.log({"example": wandb.Video("myvideo.mp4")})
```

Vous pouvez maintenant visionner des vidéos depuis le navigateur de média. Rendez-vous sur l’espace de travail de votre projet, votre espace de travail pour vos runs ou vos rapports, et cliquez sur « Ajouter visualisation » pour ajouter un panneau de médias lourds.

### Axe X personnalisé

Par défaut, nous incrémentons l’étape globale à chaque fois que vous appelez wandb.log. Si vous le souhaitez, vous pouvez enregistrer vos propres passages d’étapes monotones puis les sélectionner en tant qu’axe X personnalisé pour vos graphiques.

Par exemple, si vous avez des étapes d’entraînement et de validation que vous aimeriez aligner, envoyez-nous votre propre décompte d’étape : `wandb.log({“acc”:1, “global_step”:1})` . Puis, dans les graphiques, choisissez « global\_step » en tant qu’axe X.

`wandb.log({“acc”:1,”batch”:10}, step=epoch)`vous permettrait de choisir « batch » \(lot\) en tant qu’axe X, en plus de l’axe d’étape par défaut.

###  Naviguer et zoomer sur des nuages de points

Vous pouvez maintenir CTRL et utiliser la souris pour vous déplacer à l’intérieur de l’espace

### Il n’y a rien d’affiché sur les graphiques

Si vous voyez « Aucune donnée de visualisation enregistrée pour l’instant », cela signifie que nous n’avons pas encore reçu le premier appel de wandb.log de votre script pour l’instant. Cela peut être dû au fait que votre essai prend longtemps à finir une étape. Si vous enregistrez vos données à la fin de chaque epoch, vous pouvez essayer d’enregistrer plusieurs fois par epoch pour voir vos données arriver plus vite.

###  ****Noms de mesures dupliqués

 Si vous enregistrez différents types sous la même clef, nous devrons les séparer dans notre base de données. Cela signifie que vous verrez plusieurs entrées avec le même nom de mesure dans un menu de l’IU. Nous regroupons les types par nombre \( `number` \) , chaîne \( string \) , booléen \( `bool` \) , autres \( `other` \) \(principalement des arrays\), et tout type wandb \(`histogrammes`, `images`, etc\). Merci de n’envoyer qu’un seul type à chaque clef pour éviter cet inconvénient.

### Performances et limites

 **Échantillonnage**

Plus vous nous envoyez de points, plus cela prendra de temps pour charger vos graphiques dans l’IU. Si vous avez plus de 1 000 points sur une ligne, nous en sélectionnons un échantillon de 1 000 points de notre côté avant de renvoyer les données à votre navigateur. Cet échantillon n’est pas déterminé, ce qui signifie que si vous chargez de nouveau les informations de la page, vous verrez un ensemble différent de points.

Si vous aimeriez avoir toutes les données originales, vous pouvez utiliser [notre API](https://docs.wandb.com/library/api) pour obtenir les données non-comprises dans l’échantillon affiché.

 **Guide pratique**

 Nous vous recommandons d’essayer d’enregistrer moins de 10 000 points par mesure. Si vous avez plus de 500 colonnes de mesures de config et de sommaire, nous n’en afficherons que 500 dans le tableau. Si vous enregistrez plus d’un million de points pour une ligne, il nous faudra un certain temps pour charger la page.

Nous stockons les mesures sans traitement de la casse. Assurez-vous donc bien que deux de vos mesures n’aient pas le même nom, comme « Ma-Mesure » et « ma-mesure ».

###  Contrôler l’envoi d’images

 « Je veux intégrer W&B à mon projet, mais je ne veux envoyer aucune image »

Notre intégration n’envoie pas automatiquement des images – c’est vous qui spécifiez les fichiers que vous voulez envoyer à notre serveur de manière explicite. Voici un exemple rapide que j’ai créé pour PyTorch, où j’enregistre explicitement les images : [http://bit.ly/pytorch-mnist-colab](http://bit.ly/pytorch-mnist-colab)

```python
wandb.log({
        "Examples": example_images,
        "Test Accuracy": 100. * correct / len(test_loader.dataset),
        "Test Loss": test_loss})
```

