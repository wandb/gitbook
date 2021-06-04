# Scikit

 ****Vous pouvez utiliser wandb pour visualiser et comparer les performances de vos modèles Scikit-learn avec quelques simples lignes de code. [**Essayez cet exemple →**](https://colab.research.google.com/drive/1j_4UQTT0Lib8ueAU5zXECxesCj_ofjw7)  


###  **Faire des graphiques \(plots\)**

#### **Étape 1 : Importer wandb et initialiser un nouvel essai.**

```python
import wandb
wandb.init(project="visualize-sklearn")

# load and preprocess dataset
# train a model
```

####  **Étape 2 : Visualiser des graphiques individuellement.**

```python
# Visualize single plot
wandb.sklearn.plot_confusion_matrix(y_true, y_pred, labels)
```

**Ou visualiser tous les graphiques en une fois :**

```python
# Visualize all classifier plots
wandb.sklearn.plot_classifier(clf, X_train, X_test, y_train, y_test, y_pred, y_probas, labels,
                                                         model_name='SVC', feature_names=None)

# All regression plots
wandb.sklearn.plot_regressor(reg, X_train, X_test, y_train, y_test,  model_name='Ridge')

# All clustering plots
wandb.sklearn.plot_clusterer(kmeans, X_train, cluster_labels, labels=None, model_name='KMeans')
```

### **Graphiques pris en charge**

####  Courbe d’apprentissage

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.46.34-am.png)

Entraînez des modèles sur des jeux de données de longueurs variables et générez un graphique de scores issus d’une validation croisée avec les tailles des jeux de données, à la fois pour des sets d’entraînement et de tests.

`wandb.sklearn.plot_learning_curve(model, X, y)`

* model \(clf or reg\): accueille un régresseur ou un classifieur ajusté.
* X \(arr\): caractéristiques du jeu de données.
* y \(arr\): étiquettes du jeu de données.

#### Courbe ROC

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.02-am.png)

 Les courbes ROC tracent le taux de vrais positifs \(axe y\) contre le taux de faux positifs \(axe x\). Le score idéal est TVR = 1 et TFP = 0, ce qui est le cas tout en haut à gauche. Typiquement, on calcule l’aire sous la courbe ROC \(AUC - ROC\) et plus AUC - ROC est importante, plus le résultat est bon.

`wandb.sklearn.plot_roc(y_true, y_probas, labels)`

* y\_true \(arr\): étiquettes du set de test.
* y\_probas \(arr\): Probabilités prédites du set de test.
* labels \(list\): étiquettes nommées pour la variable cible \(y\).

#### Proportions de classe

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.46-am.png)

Graphique sur la distribution des classes cibles dans les sets d’entraînement et de test. Utile pour détecter des classes non équilibrées et s’assurer qu’une classe n’ait pas une influence disproportionnée sur le modèle.

`wandb.sklearn.plot_class_proportions(y_train, y_test, ['dog', 'cat', 'owl'])`

* y\_train \(arr\): étiquettes du set d’entraînement.
* y\_test \(arr\): étiquettes du set de test.
* labels \(list\): étiquettes nommées pour la variable cible \(y\).

#### Courbe Précision-Rappel

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.17-am.png)

Calcule le compromis entre la précision et le rappel pour différents seuils. Une aire importante sous la courbe représente à la fois un rappel élevé et une précision élevée, où une précision élevée est relative à un faible taux de faux positifs, et un rappel élevé est relatif à un faible taux de faux négatifs.

Le score maximal pour les deux valeurs montre que le classifieur retourne des résultats précis \(précision élevée\) ainsi qu’une majorité de résultats tout positifs \(rappel élevé\). La courbe PR est utile lorsque les classes sont très déséquilibrées.

`wandb.sklearn.plot_precision_recall(y_true, y_probas, labels)`

* y\_true \(arr\): étiquettes de set de test.
* y\_probas \(arr\): probabilités prédites du set de test.
* labels \(list\): étiquettes nommées pour la variable cible \(y\)

#### Importance des caractéristiques

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.48.31-am.png)

Évalue et trace l’importance de chaque caractéristique pour la tâche de classification. Ne fonctionne qu’avec des classifieurs qui ont l’attribut `feature_importances_` comme les arbres \(trees\).

`wandb.sklearn.plot_feature_importances(model, ['width', 'height, 'length'])`

* model \(clf\) : accueille un classifieur ajusté
* feature\_names \(list\) :  noms pour les caractéristiques. Rend les graphiques plus simples à lire en remplaçant les index de caractéristiques par les noms correspondants.

#### **Courbe de calibration**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.49.00-am.png)

Montre la qualité de calibration des probabilités prédites d’un classifieur et comment calibrer un classifieur non calibré. Compare les probabilités prédites estimées à un modèle de régression logistique de base, le modèle étant passé comme un argument, et par ses calibrations isotoniques et sigmoïdes.

Plus les courbes de calibrations s’apparentent à une diagonale, mieux c’est. Une courbe similaire à un sigmoïde transposé représente un classifieur surajusté, tandis qu’une courbe similaire à un sigmoïde représente un classifieur sous-ajusté. En entraînant les calibrations isotoniques et sigmoïdes du modèle et en comparant leurs courbes, on peut découvrir si le modèle est en surapprentissage ou en sous-apprentissage, et si c’est le cas, quelle calibration \(sigmoïde ou isotonique\) pourrait aider à résoudre ce problème.

Pour plus de détails, consultez la [documentation sklearn](https://scikit-learn.org/stable/auto_examples/calibration/plot_calibration_curve.html).

`wandb.sklearn.plot_calibration_curve(clf, X, y, 'RandomForestClassifier')`

* model \(clf\): accueille un classifieur ajusté.
* X \(arr\): caractéristiques du set d’entraînement.
* y \(arr\): étiquettes du set d’entraînement.
* model\_name \(str\): nom du modèle \(par défaut, 'Classifier’\)

#### Matrice de confusion

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.49.11-am.png)

Calcule la matrice de confusion pour évaluer la précision d’une classification. C’est utile pour évaluer la qualité des prédictions d’un modèle et pour trouver des schémas dans les prédictions où le modèle s’est trompé. La diagonale représente les prédictions avérées du modèle, c-à-d où l’étiquette présente est égale à l’étiquette prédite.

`wandb.sklearn.plot_confusion_matrix(y_true, y_pred, labels)`

* y\_true \(arr\): étiquettes de set de test.
* y\_pred \(arr\): étiquettes prédites de set de test.
* labels \(list\): étiquettes nommées pour la variable cible \(y\).

####  **Synthèse de métriques**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.49.28-am.png)

Calcule des mesures de sommaire \(comme f1, précision, précision et rappel pour classification et scores de mse, mae, R2 pour régression\) à la fois pour les algorithmes de régression et de classification.

`wandb.sklearn.plot_summary_metrics(model, X_train, X_test, y_train, y_test)`

* model \(clf or reg\): accueille un régresseur ou un classifieur ajusté.
* X \(arr\):  caractéristiques du set d’entraînement.
* y \(arr\): étiquettes du set d’entraînement.
  * X\_test \(arr\): caractéristiques du set de test.
* y\_test \(arr\): étiquettes du set de test.

#### **Méthode du coude**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.52.21-am.png)

Mesure et trace le pourcentage de la variance expliquée en tant que fonction du nombre de clusters \(agrégats\), avec les temps d’entraînement. Utile pour déterminer le nombre optimal de clusters \(agrégats\).

`wandb.sklearn.plot_elbow_curve(model, X_train)`

* model \(clusterer\): accueille un agrégateur \(clusterer\) ajusté.
* X \(arr\):  caractéristiques du set d’entraînement.

####  Tracé de silhouette

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.53.12-am.png)

Mesure et trace la proximité de chaque point d’un cluster \(agrégat\) avec les points des clusters voisins. L’épaisseur des clusters correspond à la taille des clusters. La ligne rouge verticale représente le score moyen de silhouette de tous les points.

 Des coefficients de silhouette proches de +1 indiquent que l’échantillon est très loin des clusters voisins. Une valeur de 0 indique que l’échantillon est sur ou très proche de la limite de décision entre deux clusters voisins, et des valeurs négatives indiquent que ces échantillons ont pu être assignés au mauvais cluster.

En général, la préférence est donnée aux scores de silhouette supérieurs à la moyenne \(au-delà de la ligne rouge\) et aussi proches que possible de 1. On privilégie également des tailles de clusters qui reflètent les schémas sous-jacents dans les données.

`wandb.sklearn.plot_silhouette(model, X_train, ['spam', 'not spam'])`

* model \(clusterer\): accueille un agrégateur \(clusterer\) ajusté.
* X \(arr\): caractéristiques du set d’entraînement.
  * cluster\_labels \(list\): noms pour les étiquettes de cluster. Rend les graphiques plus simples à lire en remplaçant les index de cluster par les noms correspondants.

####  **Graphique des données aberrantes potentielles**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.52.34-am.png)

Mesure l’influence d’une donnée sur la régression de modèle via la distance de Cook. Les instances avec des influences fortement biaisées peuvent potentiellement être des données aberrantes \(outliers\). Utile pour détecter les données aberrantes.

`wandb.sklearn.plot_outlier_candidates(model, X, y)`

* model \(regressor\): accueille un classifieur ajusté.
* X \(arr\):  caractéristiques du set d’entraînement.
* y \(arr\): étiquettes du set d’entraînement.

####  **Graphique de résidus**

![](../.gitbook/assets/screen-shot-2020-02-26-at-2.52.46-am.png)

Mesure et trace les valeurs cibles prédites \(axe y\) par rapport à la différence entre les valeurs cibles réelles et prédites \(axe x\), ainsi que la distribution des erreurs résiduelles.

Généralement, les résidus d’un modèle bien ajusté devraient être distribués aléatoirement, parce que les bons modèles prendront en compte la plupart des phénomènes dans un jeu de données, à l’exception des erreurs aléatoires.

`wandb.sklearn.plot_residuals(model, X, y)`

* model \(regressor\): accueille un classifieur ajusté.
* X \(arr\): caractéristiques du set d’entraînement.
* y \(arr\): étiquettes du set d’entraînement.

  Si vous avez des questions, nous serons ravis d’y répondre sur notre [communauté slack](http://wandb.me/slack).

## Exemple

*  [Essayez dans colab ](https://colab.research.google.com/drive/1tCppyqYFCeWsVVT4XHfck6thbhp3OGwZ): un notebook simple pour bien démarrer
*  [Tableau de Bord WandB ](https://app.wandb.ai/wandb/iris): visualisez les résultats sur W&B

