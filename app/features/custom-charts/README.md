---
description: Visuels et panneaux personnalisés en utilisant des requêtes
---

# Custom Charts

Utilisez les **graphiques personnalisés** pour créer des graphiques qui ne sont pas possibles à obtenir dans l’IU par défaut. Enregistrez des tableaux arbitraires de données et visualisez-les exactement comme vous le souhaitez. Contrôlez les détails de police, de couleur, et d’info-bulles avec le pouvoir de [Vega](https://vega.github.io/vega/).

* **Ce qui est possible** :  Lisez l’[annonce de lancement →](https://wandb.ai/wandb/posts/reports/Announcing-the-W-B-Machine-Learning-Visualization-IDE--VmlldzoyNjk3Nzg)
* **Code** : Essayer un exemple en direct dans un [notebook hébergé →](https://tiny.cc/custom-charts)
* **Video**: Essayer un exemple en direct dans un [notebook hébergé →](https://tiny.cc/custom-charts)
* **Exemple**: Rapide [notebook de démo Keras et Sklearn →](https://colab.research.google.com/drive/1g-gNGokPWM2Qbc8p1Gofud0_5AoZdoSD?usp=sharing)

 Contactez Carey \([c@wandb.com](mailto:c@wandb.com)\) pour toute question ou suggestion.

![Graphiques pris en charge depuis vega.github.io/vega](../../../.gitbook/assets/screen-shot-2020-09-09-at-2.18.17-pm.png)

### Comment ça fonctionne

1. **Enregistrez des données :** Depuis votre script, enregistrez des données de [config](https://docs.wandb.ai/library/config) et de sommaire comme vous le feriez normalement lorsque vous exécutez avec W&B. Pour visualiser une liste de valeurs multiples enregistrées à un instant spécifique, utilisez une wandb.Table personnalisée.
2.  **Personnalisez le graphique :** Sélectionnez n’importe quelle donnée avec une requête [GraphQL](https://graphql.org/). Visualisez les résultats de votre requête avec [Vega](https://vega.github.io/vega/), une grammaire de visualisation puissante.
3.  **Enregistrez le graphique** : Appelez votre propre preset depuis votre script avec `wandb.plot_table()` ou utilisez l’un de nos préconstruits intégrés.

{% page-ref page="walkthrough.md" %}

![](../../../.gitbook/assets/pr-roc.png)

## Enregistrer des graphiques depuis un script

###  Presets préconstruits intégrés

 Ces presets ont des méthodes `wandb.plot` intégrées qui rendent rapide l’enregistrement de graphiques directement depuis votre script pour obtenir les visuels exacts que vous recherchez dans l’IU.

{% tabs %}
{% tab title="Graphique linéaire" %}
`wandb.plot.line()`

Enregistrez un graphique linéaire personnalisé – une liste de points connectés et ordonnés \(x,y\) sur des axes x et y arbitraires.

```python
data = [[x, y] for (x, y) in zip(x_values, y_values)]
table = wandb.Table(data=data, columns = ["x", "y"])
wandb.log({"my_custom_plot_id" : wandb.plot.line(table, "x", "y", title="Custom Y vs X Line Plot")})
```

Vous pouvez utiliser ceci pour enregistrer des courbes dans n’importe quelles deux dimensions. Notez que si vous tracez deux listes de valeurs l’un contre l’autre, les nombres de valeurs de ces listes doivent exactement correspondre \(i.e. chaque point doit avoir un x et un y\).

![](../../../.gitbook/assets/line-plot.png)

 [Voir dans l’app →](https://wandb.ai/wandb/plots/reports/Custom-Line-Plots--VmlldzoyNjk5NTA)

 [Exécuter le code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Nuage de points" %}
`wandb.plot.scatter()`

Enregistrez un nuage de points personnalisés – une liste de points \(x,y\) sur une paire d’axes x et y arbitraires.

```python
data = [[x, y] for (x, y) in zip(class_x_prediction_scores, class_y_prediction_scores)]
table = wandb.Table(data=data, columns = ["class_x", "class_y"])
wandb.log({"my_custom_id" : wandb.plot.scatter(table, "class_x", "class_y")})
```

Vous pouvez utiliser ceci pour enregistrer des points en nuage dans n’importe quelles deux dimensions. Notez que si vous tracez deux listes de valeurs l’un contre l’autre, les nombres de valeurs de ces listes doivent exactement correspondre \(i.e. chaque point doit avoir un x et un y\).

![](../../../.gitbook/assets/demo-scatter-plot.png)

[Voir dans l’app →](https://wandb.ai/wandb/plots/reports/Custom-Scatter-Plots--VmlldzoyNjk5NDQ)

[Exécuter le code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Bar chart" %}
`wandb.plot.bar()`

Log a custom bar chart—a list of labeled values as bars—natively in a few lines:

```python
data = [[label, val] for (label, val) in zip(labels, values)]
table = wandb.Table(data=data, columns = ["label", "value"])
wandb.log({"my_bar_chart_id" : wandb.plot.bar(table, "label", "value", title="Custom Bar Chart")
```

You can use this to log arbitrary bar charts. Note that the number of labels and values in the lists must match exactly \(i.e. each data point must have both\).

![](../../../.gitbook/assets/image%20%2896%29.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Bar-Charts--VmlldzoyNzExNzk)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="Histogram" %}
`wandb.plot.histogram()`

Log a custom histogram—sort list of values into bins by count/frequency of occurrence—natively in a few lines. Let's say I have a list of prediction confidence scores \(`scores`\) and want to visualize their distribution:

```python
data = [[s] for s in scores]
table = wandb.Table(data=data, columns=["scores"])
wandb.log({'my_histogram': wandb.plot.histogram(table, "scores", title=None)})
```

You can use this to log arbitrary histograms. Note that `data` is a list of lists, intended to support a 2D array of rows and columns.

![](../../../.gitbook/assets/demo-custom-chart-histogram.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Custom-Histograms--VmlldzoyNzE0NzM)

[Run the code →](https://tiny.cc/custom-charts)
{% endtab %}

{% tab title="PR curve" %}
`wandb.plot.pr_curve()`

Log a [Precision-Recall curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_recall_curve.html#sklearn.metrics.precision_recall_curve) in one line:

```python
wandb.log({"pr" : wandb.plot.pr_curve(ground_truth, predictions,
                     labels=None, classes_to_plot=None)})
```

You can log this whenever your code has access to:

* a model's predicted scores \(`predictions`\) on a set of examples
* the corresponding ground truth labels \(`ground_truth`\) for those examples
* \(optionally\) a list of the labels/class names \(`labels=["cat", "dog", "bird"...]` if label index 0 means cat, 1 = dog, 2 = bird, etc.\)
* \(optionally\) a subset \(still in list format\) of the labels to visualize in the plot

![](../../../.gitbook/assets/demo-precision-recall.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-Precision-Recall-Curves--VmlldzoyNjk1ODY)

[Run the code →](https://colab.research.google.com/drive/1mS8ogA3LcZWOXchfJoMrboW3opY1A8BY?usp=sharing)
{% endtab %}

{% tab title="ROC curve" %}
`wandb.plot.roc_curve()`

Log an [ROC curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html#sklearn.metrics.roc_curve) in one line:

```text
wandb.log({"roc" : wandb.plot.roc_curve( ground_truth, predictions, \
                        labels=None, classes_to_plot=None)})
```

You can log this whenever your code has access to:

* a model's predicted scores \(`predictions`\) on a set of examples
* the corresponding ground truth labels \(`ground_truth`\) for those examples
* \(optionally\) a list of the labels/ class names \(`labels=["cat", "dog", "bird"...]` if label index 0 means cat, 1 = dog, 2 = bird, etc.\)
* \(optionally\) a subset \(still in list format\) of these labels to visualize on the plot

![](../../../.gitbook/assets/demo-custom-chart-roc-curve.png)

[See in the app →](https://wandb.ai/wandb/plots/reports/Plot-ROC-Curves--VmlldzoyNjk3MDE)

[Run the code →](https://colab.research.google.com/drive/1_RMppCqsA8XInV_jhJz32NCZG6Z5t1RO?usp=sharing)
{% endtab %}
{% endtabs %}

###  ****Graphique en barres

 Enregistrez un graphique en barres personnalisé – une liste de valeurs libellées sous forme de barres – nativement en quelques lignes :

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

![](../../../.gitbook/assets/image%20%2897%29.png)

## Log data

Enregistrez un nuage de points personnalisés – une liste de points \(x,y\) sur une paire d’axes x et y arbitraires.

* **Config**: Initial settings of your experiment \(your independent variables\). This includes any named fields you've logged as keys to `wandb.config` at the start of your training \(e.g. `wandb.config.learning_rate = 0.0001)` 
* **Summary**: Single values logged during training \(your results or dependent variables\), e.g. `wandb.log({"val_acc" : 0.8})`. If you write to this key multiple times during training via `wandb.log()`, the summary is set to the final value of that key.
* **History**: The full timeseries of the logged scalar is available to the query via the `history` field
* **summaryTable**: If you need to log a list of multiple values, use a `wandb.Table()` to save that data, then query it in your custom panel.

### **How to log a custom table**

Use `wandb.Table()` to log your data as a 2D array. Typically each row of this table represents one data point, and each column denotes the relevant fields/dimensions for each data point which you'd like to plot. As you configure a custom panel, the whole table will be accessible via the named key passed to `wandb.log()`\("custom\_data\_table" below\), and the individual fields will be accessible via the column names \("x", "y", and "z"\). You can log tables at multiple time steps throughout your experiment. The maximum size of each table is 10,000 rows.

[Try it in a Google Colab →](https://tiny.cc/custom-charts)

```python
# Logging a custom table of data
my_custom_data = [[x1, y1, z1], [x2, y2, z2]]
wandb.log({“custom_data_table”: wandb.Table(data=my_custom_data,
                                columns = ["x", "y", "z"])})
```

##  Comment éditer Vega

Cliquez sur **Éditer** en haut du panneau pour vous rendre dans le mode édition de [Vega](https://vega.github.io/vega/). Ici, vous pouvez définir une [spécification Vega](https://vega.github.io/vega/docs/specification/) qui crée un graphique interactif dans l’IU. Vous pouvez changer n’importe quel aspect du graphique, depuis le style visuel \(e.g. changer le titre, choisir un schéma de couleurs différent, montrer les courbes comme une série de points plutôt que des lignes connectées\) jusqu’aux données en elles-mêmes \(utilisez un Vega transform pour regrouper un array de valeurs en un histogramme, etc.\). La prévisualisation de panneau \(panel preview\) se mettra à jour de manière interactive, pour que vous puissiez voir les effets de vos modifications tout en éditant les specs Vega ou votre requête. La [documentation et les tutoriels Vega](https://vega.github.io/vega/) sont une excellente source d’inspiration.

![Add a new custom chart, then edit the query](../../../.gitbook/assets/2020-08-28-06.42.40.gif)

### Visuels personnalisés

Sélectionnez un **Graphique** \(Chart\) dans le coin en haut à droite pour commencer avec un preset par défaut. Puis, choisissez **Champs de Graphique** \(Chart Fields\) pour mapper les données que vous obtenez de votre requête sur les champs correspondants dans votre graphique. Voici un exemple de sélection d’une mesure à obtenir depuis la requête, puis son mappage dans les champs de graphique en barres, ci-dessous.

![Cr&#xE9;ation d&#x2019;un graphique en barres personnalis&#xE9; qui montre la pr&#xE9;cision sur diff&#xE9;rents essais d&#x2019;un projet](../../../.gitbook/assets/demo-make-a-custom-chart-bar-chart.gif)

### Comment éditer Vega

Cliquez sur **Éditer** en haut du panneau pour vous rendre dans le mode édition de [Vega](https://vega.github.io/vega/). Ici, vous pouvez définir une [spécification Vega](https://vega.github.io/vega/docs/specification/) qui crée un graphique interactif dans l’IU. Vous pouvez changer n’importe quel aspect du graphique, depuis le style visuel \(e.g. changer le titre, choisir un schéma de couleurs différent, montrer les courbes comme une série de points plutôt que des lignes connectées\) jusqu’aux données en elles-mêmes \(utilisez un Vega transform pour regrouper un array de valeurs en un histogramme, etc.\). La prévisualisation de panneau \(panel preview\) se mettra à jour de manière interactive, pour que vous puissiez voir les effets de vos modifications tout en éditant les specs Vega ou votre requête. La [documentation et les tutoriels Vega](https://vega.github.io/vega/) sont une excellente source d’inspiration.

**Références de champ**

Pour tirer des données de vos graphiques depuis W&B, ajoutez des chaînes de données au format `"${field:<field-name>}" n’importe où dans vos specs Vega. Cela créera un menu déroulant dans la zone de` **`Champs de Graphique`** `(Chart Fields) sur le côté droit, que les utilisateurs peuvent utiliser pour sélectionner une colonne de résultats de requête à mapper dans Vega.`

`Pour paramétrer une valeur par défaut pour un champ, utilisez la syntaxe` "${field:&lt;field-name&gt;:&lt;placeholder text&gt;}"

Pour paramétrer une valeur par défaut pour un champ, utilisez la syntaxe `"${field:<field-name>:<placeholder text>}`

### Enregistrer les presets de graphique

Appliquez tous les changements d’un panneau de visualisation spécifique avec le bouton en bas du modal. De manière alternative, vous pouvez sauvegarder les specs Vega à utiliser ailleurs dans votre projet. Pour sauvegarder la définition de graphique réutilisable, cliquez sur **Enregistrer sous** \(Save as\) en haut de l’éditeur Vega et donnez un nom à votre preset.

## Articles et guides

1. [L’EDI de Visualisation d’Apprentissage Automatique de W&B](https://wandb.ai/wandb/posts/reports/The-W-B-Machine-Learning-Visualization-IDE--VmlldzoyNjk3Nzg)
2.  [Visualiser des Modèles Basés sur l’Attention au TAL](https://wandb.ai/kylegoyette/gradientsandtranslation2/reports/Visualizing-NLP-Attention-Based-Models-Using-Custom-Charts--VmlldzoyNjg2MjM)
3. [Visualiser les Effets de l’Attention sur le Flux de Dégradés](https://wandb.ai/kylegoyette/gradientsandtranslation/reports/Visualizing-The-Effect-of-Attention-on-Gradient-Flow-Using-Custom-Charts--VmlldzoyNjg1NDg)
4. [Enregistrer des courbes arbitraires](https://wandb.ai/stacey/deep-drive/reports/Logging-arbitrary-curves--VmlldzoyMzczMjM)

## Foire aux Questions

###  Bientôt disponible

*    **Sondages :** Rafraîchir automatiquement les données dans le graphique
*  **Échantillonnages :** Ajuster dynamiquement le nombre de points total chargé dans le panneau pour plus d’efficacité

### Astuces

*   Vous ne voyez pas les données auxquelles vous vous attendiez dans la requête pendant que vous éditez votre graphique ? C’est peut-être parce que la colonne que vous recherchez n’est pas enregistrée dans les essais que vous avez sélectionnés. Sauvegardez votre graphique et retournez aux tableaux d’essais, puis sélectionnez les essais que vous aimeriez voir avec l’icône d’**œil**.

#### Utilisations fréquentes

* Personnaliser des graphiques en barres avec des barres d’erreur
* Montrer les mesures de validation d’un modèle qui requiert des coordonnées x-y personnalisées \(comme les courbes précision-rappel\)
* Superposer les distributions de données de deux expériences/modèles différents sous forme d’histogrammes
* Montrer les changements dans une mesure via des instantanés à plusieurs points dans l’entraînement
* Créer un visuel unique qui n’est pas encore disponible dans W&B \(et avec un peu de chance, le partager avec le monde entier\)

    ****

