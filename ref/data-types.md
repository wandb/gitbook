---
description: wandb.data_types
---

# Data Types Reference

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L0)

Wandb a des types de données spécifiques pour enregistrer des visuels riches.

Tous les types de données spécifiques sont des sous-classes de WBValue. Tous les types de données se sérialise sur JSON, puisque c’est ce que wandb utilise pour sauvegarder des objets de manière locale avant de les télécharger sur le serveur W&B.

## WBValue

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L43)

```python
WBValue(self)
```

Classe parent abstraite pour des éléments qui peuvent être enregistrés par wandb.log\(\) et visualisés par wandb.

Les objets seront sérialisés sous JSON et auront toujours un attribut \_type qui indique comment interpréter les autres champs.

**Renvoie :**

Une représentation  `dict` compatible avec JSON de cet objet, qui peut ensuite être sérialisé en chaîne de données \(string\).

## Histogramme

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L64)

```python
Histogram(self, sequence=None, np_histogram=None, num_bins=64)
```

classe wandb pour les histograms

Cet objet fonctionne exactement comme la fonction numpy histogram [https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html](https://docs.scipy.org/doc/numpy/reference/generated/numpy.histogram.html)

**Exemples :**

Génère un histogramme depuis une séquence

```python
wandb.Histogram([1,2,3])
```

Initialise efficacement depuis np.histogram.

```python
hist = np.histogram(data)
wandb.Histogram(np_histogram=hist)
```

**Arguments**:

* `sequence` _array\_like_ - input des données pour l’histogramme
* `np_histogram` _numpy histogram_ - input alternatif d’un histogramme pré-calculé
* `num_bins` _int_ - Nombre de regroupements pour l’histogramme. Par défaut, le nombre de regroupements est 64. Le nombre maximum de regroupements est 512

**Attributs :**

* `bins` _\[float\]_ - bords des regroupements
* `histogram` _\[int\]_ - nombre d’éléments qui se trouve dans chaque regroupement

## Médias

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L122)

```python
Media(self, caption=None)
```

Une WBValue que nous stockons comme un fichier extérieur à JSON et qui montre un panneau média sur le front end.

Si nécessaire, nous déplaçons ou copions le fichier dans le répertoire de médias du Run pour qu’il soit mis en ligne.

## BatchableMedia

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L232)

```python
BatchableMedia(self, caption=None)
```

Classe parent pour Média que nous traitons spécialement en lots \(batch\) comme des images ou des miniatures.

À part les images, nous utilisons ces lots pour aider à organiser les fichiers par nom dans le répertoire média.

### Tableau

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L244)

```python
Table(self,
      columns=['Input', 'Output', 'Expected'],
      data=None,
      rows=None,
      dataframe=None)
```

C’est un tableau fait pour afficher de petits sets d’enregistrement.

**Arguments**:

* `columns` _\[str\]_ - Noms des colonnes du tableau. Par défaut, \["Input", "Output", "Expected"\].
* `data` _array_ - Array 2D de valeurs qui seront affichées comme chaînes de données. 
* `dataframe` _pandas.DataFrame_ - Objet Dataframe utilisé pour créer ce tableau. Lorsqu’il est réglé, les autres arguments sont ignorés.

## Audio

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L305)

```python
Audio(self, data_or_path, sample_rate=None, caption=None)
```

Classe Wandb pour les clips audio.

**Arguments**:

* `data_or_path` _chaîne ou numpy array_ – Un chemin vers un fichier audio ou un numpy array de données audio.
* `sample_rate` _int_ - Taux d’échantillonnage, requis lorsqu’on passe un numpy array de données audio en brut.
* `caption` _string_ - Libellé à afficher avec l’audio.

## Object3D

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L404)

```python
Object3D(self, data_or_path, **kwargs)
```

Classe Wandb pour les points de nuages 3D.

**Arguments**:

data\_or\_path \(numpy array \| string \| io \): Object3D peut être initialisé depuis un fichier ou un numpy array.

Les formats de fichier pris en charge sont obj, gltf, babylon, stl. Vous pouvez passer un chemin à un fichier ou à un objet io et un file\_type \(type de fichier\) qui doit être l’un des suivants `'obj', 'gltf', 'babylon', 'stl'`

La forme du numpy array doit être l’une des suivantes :

```python
[[x y z],       ...] nx3
[x y z c],     ...] nx4 where c is a category with supported range [1, 14]
[x y z r g b], ...] nx4 where is rgb is color
```

## Molecule

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L527)

```python
Molecule(self, data_or_path, **kwargs)
```

Classe Wandb pour les données Moléculaires

**Arguments**:

data\_or\_path \( string \| io \): Molecule peut être initialisé depuis un nom de fichier ou un objet io.

## Html

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L611)

```python
Html(self, data, inject=True)
```

Classe Wandb pour html arbitraire

**Arguments**:

* `data` _chaîne ou objet io_ - _TML à afficher dans wandb._
* `inject` _booléen_ – Ajoute une fiche de style \(stylesheet\) à l’objet HTML. Si réglé sur False, l’HTML passera sans être changé.

## Vidéo

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L680)

```python
Video(self, data_or_path, caption=None, fps=4, format=None)
```

Représentation Wandb de video.

**Arguments**:

data\_or\_path \(numpy array \| chaîne \| io\): Video peut être initialisé avec un chemin vers un fichier ou un objet io. Le format doit être "gif", "mp4", "webm" ou "ogg". Le format doit être spécifié avec l’argument de format. Video peut être initialisé par un tenseur numpy. Le tenseur numpy doit comporter soit 4 soit 5 dimensions. Les canaux doivent être \(time, channel, height, width\) ou \(batch, time, channel, height, width\) – \(\(lot, temps, canal, hauteur, largeur\)\).

* `caption` _chaîne_ – Libellé associé avec la vidéo pour affichage
* `fps` _int_ - _int_ – images par seconde de vidéo. Par défaut, 4.
* `format` _chaîne_ – format de vidéo, nécessaire si initialisation par chemin ou objet io.

## Image

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L827)

```python
Image(self,
      data_or_path,
      mode=None,
      caption=None,
      grouping=None,
      boxes=None,
      masks=None)
```

 Classe Wandb pour les images.

**Arguments**:

* `data_or_path` _numpy array \| chaîne \| io_ – Accepte les numpy array de données d’images, ou une image PIL. La classe essaye d’inférer le format de données et de le convertir.
* `mode` _chaîne_ – Le mode PIL pour une image. Les plus communs sont "L", "RGB", "RGBA". Explication complète à [https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html\#concept-modes](https://pillow.readthedocs.io/en/4.2.x/handbook/concepts.html#concept-modes).
* `caption` c_haîne_ – Libellé pour l’affichage de l’image.

## JSONMetadata

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1093)

```python
JSONMetadata(self, val, **kwargs)
```

JSONMetadata est un type pour encoder des métadonnées arbitraires sous forme de fichiers.

## BoundingBoxes2D

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1126)

```python
BoundingBoxes2D(self, val, key, **kwargs)
```

Classe Wandb pour les boîtes à limite minimum 2D.

## ImageMask

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1204)

```python
ImageMask(self, val, key, **kwargs)
```

Classe Wandb pour les masques d’images, utile pour les tâches de segmentation.

## Plotly

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1274)

```python
Plotly(self, val, **kwargs)
```

 Classe Wandb pour les graphiques plotly.

**Arguments**:

* `val` - figure matplotlib ou plotly

## Graph

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1314)

```python
Graph(self, format='keras')
```

 Classe Wandb pour les graphiques

Cette classe est typiquement utilisée pour sauvegarder et afficher les modèles de filet neuronaux. Elle représente le graphique comme un array de nœuds \(nodes\) et d’arêtes \(edges\). Les nœuds peuvent avoir des libellés visionnables dans wandb.

**Exemples :**

 Import d’un modèle keras :

```python
Graph.from_keras(keras_model)
```

**Attributs :**

* `format` _chaîne_ – Format pour aider à wandb à afficher joliment le graphique.
* `nodes` _\[wandb.Node\]_ - Liste de wandb.Nodes
* `nodes_by_id` _dict_ -dict of ids -&gt; nodes edges \(\[\(wandb.Node, wandb.Node\)\]\): Liste de paires de nodes interprétées comme des arêtes.
* `loaded` _boolean_ - Flag pour indiquer si le graphique est complétement chargé
* `root` _wandb.Node_ - _Nœud central du graphique \(root node\)_

## Node

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1470)

```python
Node(self,
     id=None,
     name=None,
     class_name=None,
     size=None,
     parameters=None,
     output_shape=None,
     is_output=None,
     num_parameters=None,
     node=None)
```

Nœud utilisé dans [`Graph`](data-types.md#graph)

## Edge

[source](https://github.com/wandb/client/blob/master/wandb/data_types.py#L1636)

```python
Edge(self, from_node, to_node)
```

Arête utilisée dans [`Graph`](data-types.md#graph)

