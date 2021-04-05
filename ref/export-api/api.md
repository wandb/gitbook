---
description: wandb.apis.public
---

# Public API Reference

## wandb.apis.public

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1)

### Api Objects

```python
class Api(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L181)

Utilisé pour faire une requête au serveur wandb.

**Exemples :**

Manière la plus fréquente d’initialiser

```text
wandb.Api()
```

**Arguments**:

* `overrides` _dict_ - _dict_ – Vous pouvez paramétrer `base_url` si vous utiliser un serveur wandb autre que [https://api.wandb.ai](https://api.wandb.ai/).

  Vous pouvez aussi paramétrer les valeurs par défaut pour `entity`, `project`, et `run`.

**flush**

```python
 | flush()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L276)

L’objet api garde un cache local d’essais, donc, si l’état de l’essai risque de changer pendant l’exécution de votre script, vous devez vider le cache local avec `api.flush()pour obtenir les dernières valeurs associées avec l’essai.`

**projects**

```python
 | projects(entity=None, per_page=200)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L338)

Obtient les projets pour une entité donnée.

**Arguments**:

* `entity` _str_ - Nom de l’entité requise. Si None, retournera à une entité par défaut passée dans `Api`. Si aucune entité par défaut, soulèvera une `ValueError`.
* `per_page` _int_ - Paramètre la taille de page pour la requête de pagination. None utilisera la taille par défaut. Généralement, il n’y a aucune raison de la modifier.

 **Renvoie :**

Un objet `Projects qui est une collection itérative d’objets Project.`

**reports**

```python
 | reports(path="", name=None, per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L360)

Obtient les rapports pour un chemin de projet donné.

ATTENTION : Cette api est en beta, et changera probablement dans un déploiement prochain.

**Arguments**:

* `path` _str_ - chemin vers le projet où se trouve le rapport, doit être sous la forme : "entity/project"
* `name` _str_ - nom optionnel pour le rapport requis.
* `per_page` _int_ - Paramètre la taille de page pour la requête de pagination. None utilisera la taille par défaut. Généralement, il n’y a aucune raison de la modifier.

**Renvoie :**

 Un objet Reports qui est une collection itérative d’objets `BetaReport`.

**runs**

```python
 | runs(path="", filters={}, order="-created_at", per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L393)

Return a set of runs from a project that match the filters provided. You can filter by `config.*`, `summary.*`, `state`, `entity`, `createdAt`, etc. Renvoie un set d’essais depuis un projet qui correspond aux filtres fournis. Vous pouvez filtrer par `config.,` _`summary.`_,   `state, entity,` `createdAt,` etc.

 **Exemples :**

Trouver des essais dans my\_project config.experiment\_name a été réglé sur "foo"

```text
api.runs(path="my_entity/my_project", {"config.experiment_name": "foo"})
```

Trouver des essais dans my\_project config.experiment\_name a été réglé sur "foo" ou "bar"

```text
api.runs(path="my_entity/my_project",
- `{"$or"` - [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
```

Trouver dans my\_project sorted par pertes croissantes \(ascending loss\).

```text
api.runs(path="my_entity/my_project", {"order": "+summary_metrics.loss"})
```

**Arguments**:

* `path` _str_ - chemin jusqu’au projet, doit être sous la forme : "entity/project"
* `filters` _dict_ – requêtes pour des essais spécifiques utilisant le langage de requête MongoDB. Vous pouvez filtrer par propriétés d’essais telles que config.key, summary\_metrics.key, state, entity, createdAt, etc.

  **Par exemple : {"config.experiment\_name": "foo"} trouvera des essais avec une config entry pour les noms d’expériences paramétrés sur "foo"Vous pouvez composer des opérations pour effectuer des requêtes plus complexes, voir Reference pour le langage sur** [**https://docs.mongodb.com/manual/reference/operator/query**](https://docs.mongodb.com/manual/reference/operator/query)\*\*\*\*

* `order` _str_ -  L’ordre peut être `created_at`, `heartbeat_at`, `config.*.value`, ou `summary_metrics.*`.Si vous ajoutez un + avant order, l’ordre est croissant.Si vous ajouter un – avant order, l’ordre est décroissant \(par défaut\).L’ordre par défaut est run.created\_at du plus récent au plus ancien.

**Renvoie :**

Un objet `Runs` qui est une collection itérative d’objets `Run`.

**run**

```python
 | @normalize_exceptions
 | run(path="")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L445)

Renvoie un run unique en analysant un chemin sous la forme entity/project/run\_id.

**Arguments**:

* `path` _str_ - chemin à executer sous la forme entity/project/run\_id.Si api.entity est paramétré, peut être sous la forme project/run\_idet si api.project est paramétré, peut être simplement run\_id.

 **Renvoie :**

Un object `Run` 

**sweep**

```python
 | @normalize_exceptions
 | sweep(path="")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L462)

Renvoie un balayage en analysant un chemin sous la forme entity/project/sweep\_id.

**Arguments**:

* `path` str, optionnel – chemin jusqu’au balayage, sous la forme entity/project/sweep\_id.Si api.entity est paramétré, peut être sous le forme project/sweep\_id et si api.project est paramétré, peut être simplement sweep\_id.

**Renvoie :**

Un objet `Sweep` 

**artifact**

```python
 | @normalize_exceptions
 | artifact(name, type=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L496)

Renvoie un artefact unique en analysant un chemin sous la forme entity/project/run\_id.

**Arguments**:

* `name` _str_ - Un nom d’artefact. Peut être précédé par entity/project. Noms valides sous les formes suivantes :name:versionname:aliasdigest
* `type` _str, optional_ - Le type d’artefacts à ramener.

 **Renvoie :**

 Un objet Artifact.

###  **Objets Project**

```python
class Projects(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L616)

An iterable collection of `Project` objects.

### Project Objects

```python
class Project(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L678)

 Un projet est un espace de nom \(namespace\) pour les essais.

### **Objets Runs**

```python
class Runs(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L699)

 Une collection iterative de runs associés avec un projet et un filtre optionnel. Généralement utilisé de manière indirecte par la méthode `Api.`runs.

### Run Objects

```python
class Run(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L804)

Un essai unique associé à une entité et un projet.

**Attributes**:

* `tags` _\[str\]_ - une liste d’étiquettes \(tags\) associée avec l’essai
* `url` _str_ - l’url de cet essai
* `id` _str_ - identifiant unique pour l’essai \(par défaut, huit caractères\)
* `name` _str_ - le nom de l’essai
* `state` _str_ - un parmi : running, finished, crashed, aborted
* `config` _dict_ - un dict d’hyperparamètres associés avec l’essai
* `created_at` _str_ - timestamp ISO de début de l’essai
* `system_metrics` _dict_ - les dernières mesures de système enregistrées pour l’essai
* `summary` _dict_ -Une propriété mutable semblable à un dictionnaire qui tient le sommaire actuel. Appeler update fera persister tout changement.
* `project` _str_ - le projet associé à cet essai
* `entity` _str_ - le projet associé à cet essai
* `user` _str_ - le nom de l’entité associée à cet essai
* `path` _str_ - le nom de l’utilisateur qui a créé cet essai
* `notes` _str_ - Notes sur l’essai
* `read_only` _boolean_ - L’essai est ou non modifiable
* `history_keys` _str_ - Clefs des mesures de l’historique qui ont été enregistrées avec `wandb.log({key: value})`

**\_\_init\_\_**

```python
 | __init__(client, entity, project, run_id, attrs={})
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L829)

Un essai est toujours initialisé en appelan api.runs\(\) où api est une instance de wandb.Api

**create**

```python
 | @classmethod
 | create(cls, api, run_id=None, project=None, entity=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L887)

Crée un essai pour le projet donné

**update**

```python
 | @normalize_exceptions
 | update()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L993)

Fait persister les changements de l’objet run dans le backend wandb.

**files**

```python
 | @normalize_exceptions
 | files(names=[], per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1070)

**Arguments**:

* `names` _list_ - noms des fichiers requis, si vide, retourne tous les fichiers
* `per_page` _int_ - nombre de résultats par page

 **Renvoie :**

 Un objet `Files`, qui est un itérateur sur les objets `File`.

**file**

```python
 | @normalize_exceptions
 | file(name)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1082)

**Arguments**:

* `name` str – nom du fichier demandé.

**Returns**:

 Un `File` qui corresponde à l’argument name.

**upload\_file**

```python
 | @normalize_exceptions
 | upload_file(path, root=".")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1093)

**Arguments**:

* `path` _str_ - nom du fichier à envoyer.
* `root` _str_ -le chemin root dans lequel sauvegarder le fichier relatif. i.e.Si vous voulez avoir un fichier sauvegardé dans l’essai comme  "my\_dir/file.txt" et que vous êtes dans "my\_dir", il faudra régler votre root sur "../"

 **Renvoie :**

 Un `File` qui corresponde à l’argument name.

**history**

```python
 | @normalize_exceptions
 | history(samples=500, keys=None, x_axis="_step", pandas=True, stream="default")
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1116)

Renvoie les mesures d’historique échantillonées pour un essai. C’est plus simple et plus rapide si ça vous convient que les enregistrements d’historique soient échantillonnés.

**Arguments**:

* `samples` _int, optional_ - Le nombre d’échantillons à renvoyer
* `pandas` _bool, optional_ - Renvoie une dataframe pandas
* `keys` _list, optional_ - Ne renvoie que les mesures pour des clefs spécifiques
* `x_axis` _str, optional_ - Utilisez cette mesure comme le défaut pour xAxis pour \_step
* `stream` _str, optional_ - "default" pour les mesures, "system" pour les mesures de machines

 **Renvoie :**

 Si pandas=True, renvoie une `pandas.DataFrame` de mesures d’historique. Si pandas=False, renvoie une liste de dicts de mesures d’historique.

**scan\_history**

```python
 | @normalize_exceptions
 | scan_history(keys=None, page_size=1000, min_step=None, max_step=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1150)

Renvoie une collection itérative de tous les enregistrements d’historique pour un essai.

**Exemple :**

Exporter toutes les pertes de valeur pour un essai d’exemple

```python
run = api.run("l2k2/examples-numpy-boston/i0wt6xua")
history = run.scan_history(keys=["Loss"])
losses = [row["Loss"] for row in history]
```

**Arguments**:

* `keys`str\], optionnel – ne ramène que ces clefs, et ne ramène que les lignes qui ont toutes ces clefs définies.
* `page_size` int, optionnel – taille des pages à ramener depuis l’api

**Renvoie :**

Une collection itérative au-dessus d’enregistrements d’historiques \(dict\).

**use\_artifact**

```python
 | @normalize_exceptions
 | use_artifact(artifact)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1207)

Déclare un artefact comme input d’un essai.

**Arguments**:

* `artifact` _`Artifact`_ - Un artefact renvoyé depuis

  `wandb.Api().artifact(name)`

**Renvoie :**

Un objet `Artifact` 

**log\_artifact**

```python
 | @normalize_exceptions
 | log_artifact(artifact, aliases=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1234)

 Déclare un artefact comme output d’un essai.

**Arguments**:

* `artifact` _`Artifact`_ - Un artefact renvoyé depuis

  `wandb.Api().artifact(name)`

* `aliases` list, optionnel - Alias à appliquer à cet artefact

**Renvoie :**

Un objet`Artifact` 

### Sweep Objects

```python
class Sweep(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1314)

Un ensemble d’essais associés avec un balayage Instantié avec : api.sweep\(sweep\_path\)

**Attributes**:

* `runs` _`Runs`_ -liste d’essais
* `id` _str_ - id du balayage
* `project` _str_ - nom du projet
* `config` _str_ - dictionnaire de configuration de balayage

**best\_run**

```python
 | best_run(order=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1400)

Renvoie le meilleur essai classé par la mesure définie dans la config ou l’order indiqué

**get**

```python
 | @classmethod
 | get(cls, client, entity=None, project=None, sid=None, withRuns=True, order=None, query=None, **kwargs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1440)

 Files est une collection itérative d’objets File.

### Files Objects

```python
class Files(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1495)

`File` est une classe associée avec un fichier sauvegardé par wandb.

### File Objects

```python
class File(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1561)

File is a class associated with a file saved by wandb.

**Attributes**:

* `name` _string_ -nom de fichier \(filename\)
* `url` _string_ - chemin vers le fichier
* `md5` _string_ - md5 du fichier
* `mimetype` _string_ - mimetype du fichier
* `updated_at` _string_ - timestamp de la dernière mise à jour
* `size` _int_ - taille du fichier en bytes

**download**

```python
 | @normalize_exceptions
 | @retriable(
 |         retry_timedelta=RETRY_TIMEDELTA,
 |         check_retry_fn=util.no_retry_auth,
 |         retryable_exceptions=(RetryError, requests.RequestException),
 |     )
 | download(root=".", replace=False)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1618)

 Télécharge un fichier précédemment sauvegardé par un essai sur le serveur wandb.

**Arguments**:

* `replace` _boolean_ - Si `True`, le téléchargement écrasera un fichier local s’il existe. par défaut, `False`.
* `root` _str_ - Répertoire local dans lequel sauvegarder le fichier. Par défaut, ".".

**Raises**:

`ValueError` si le fichier existe déjà et que replace=False

### Reports Objects

```python
class Reports(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1641)

Reports est une collection iterative d’objets `BetaReport` 

###  Objets QueryGenerator 

```python
class QueryGenerator(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1721)

QueryGenerator est un objet helper pour écrire des filtres pour les essais.

###  Objets BetaReport

```python
class BetaReport(Attrs)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L1818)

BetaReport est une classe associée avec les rapports créés avec wandb.

ATTENTION : cette API changera sûrement dans un déploiement futur.

**Attributes**:

* `name` chaîne – nom du rapport
* `description` chaîne – description du rapport
* `user` User – l’utilisateur qui a créé le rapport
* `spec` _d_ict – les spec du rapport
* `updated_at` chaîne - timestamp de la dernière mise à jour

### **Objets ArtifactType**

```python
class ArtifactType(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2282)

**collections**

```python
 | @normalize_exceptions
 | collections(per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2337)

 Collections d’artefacts.

###  ArtifactCollection Objects

```python
class ArtifactCollection(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2352)

**versions**

```python
 | @normalize_exceptions
 | versions(per_page=50)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2366)

 Versions d’artefact

###  Objets Artifact 

```python
class Artifact(object)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2381)

**delete**

```python
 | delete()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2534)

 Supprime un artefact et ses fichiers.

**get**

```python
 | get(name)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2628)

 Renvoie la resource de wandb.Media stockée dans l’artefact. Les medias peuvent être stockés dans un artefact via Artifact\#add\(obj: wandbMedia, name: str\)\`

**Arguments**:

* `name` str – nom de la ressource

 **Renvoie :**

Un `wandb.Media` qui a été stocké à `name`

**download**

```python
 | download(root=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2663)

 Télécharge l’artefact dans le répertoire spécifié par les

**Arguments**:

* `root` str, optionnel – répertoire dans lequel télécharger l’artefact. Si None, l’artefact sera téléchargé dans './artifacts//'

 **Renvoie :**

Le chemin vers le contenu du téléchargement.

**file**

```python
 | file(root=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2702)

Télécharge un fichier d’artefact unique dans le répertoire spécifié par les

**Arguments**:

* `root` str, optionnel – répertoire dans lequel télécharger l’artefact. Si None, l’artefact sera téléchargé dans './artifacts//'

 **Renvoie :**

 Le chemin intégral du fichier téléchargé.

**save**

```python
 | @normalize_exceptions
 | save()
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2737)

Fait persister les changements d’artefact sur le backend wandb.

**verify**

```python
 | verify(root=None)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2776)

Vérifie un artefact en faisant une somme de contrôle des contenus téléchargés.

Soulève une ValueError si la vérification échoue. Ne vérifie pas les fichiers de références téléchargés.

**Arguments**:

* `root` str, optionnel – répertoire dans lequel télécharger l’artefact. Si None, l’artefact sera téléchargé dans './artifacts//'

### Objets ArtifactVersions

```python
class ArtifactVersions(Paginator)
```

[\[source\]](https://github.com/wandb/client/blob/21787ccda9c60578fcf0c7f7b0d06c887b48a343/wandb/apis/public.py#L2930)

Une collection itérative de versions d’artefacts associés avec un projet et un filtre optionnel. Généralement utilisé de manière indirecte par la méthode `Api`.artifact\_versions 

