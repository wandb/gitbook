# Run

## wandb.sdk.wandb\_run

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L4)

### Run Objects

```python
class Run(object)
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L131)

L’objet run correspond à une seule exécution de votre script, typiquement, ceci est une expérience d’apprentissage automatique \(ML\). Créez un run \(essai\) avec wandb.init\(\).

Dans l’entraînement distribué, utilisez wandb.init\(\) pour créer un essai pour chaque processus, et paramétrez l’argument de groupe pour organiser des essais dans une expérience plus grande.

**Attributes**:

* `history` _`History`_ - Valeurs de temps de séries, créées avec wandb.log\(\). L’historique peut contenir des valeurs scalaires, des médias lourds, et même des graphiques personnalisés sur plusieurs étapes.
* `summary` _`Summary`_ - Set unique de valeurs pour chaque clef wandb.log\(\). Par défaut, sommaire est égal à la dernière valeur enregistrée. Vous pouvez manuellement régler le sommaire sur la meilleure valeur, comme la précision max, plutôt que sur la valeur finale.

**dir**

```python
 | @property
 | dir()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L333)

str: : Le répertoire où tous les fichiers associés à l’essai sont placés.

**config**

```python
 | @property
 | config()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L340)

\(`Config`\): Un objet config \(similaire à un dict imbriqué\) de paires de valeur clefs associées aux hyperparamètres de l’essai.

**name**

```python
 | @property
 | name()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L351)

str: le nom affiché de l’essai. Il n’a pas besoin d’être unique et, dans l’idéal, est descriptif.

**notes**

```python
 | @property
 | notes()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L367)

str: notes associées à cet essai. Les notes peuvent être une chaîne multilignes et peuvent aussi être utilisées dans des équations de markdown et de latex à l’intérieur de $$ comme ${x}

**tags**

```python
 | @property
 | tags() -> Optional[Tuple]
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L383)

Tuple \[str\] : étiquettes associées à cet essai

**id**

```python
 | @property
 | id()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L397)

str: le run\_id associée à cet essai

**sweep\_id**

```python
 | @property
 | sweep_id()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L402)

\(str, optionnel\) : l’id de balayage associée avec cet essai, ou Aucun \(None\)

**path**

```python
 | @property
 | path()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L409)

str : le chemin jusqu’à l’essai \[entity\]/\[project\]/\[run\_id\]

**start\_time**

```python
 | @property
 | start_time()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L418)

int : time stamp unix en secondes du moment où l’essai commence

**starting\_step**

```python
 | @property
 | starting_step()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L426)

int : la première étape de l’essai

**resumed**

```python
 | @property
 | resumed()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L434)

 bool : l’essai a ou n’a pas été repris

**step**

```python
 | @property
 | step()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L442)

int : compteur d’étapes

Chaque fois que vous appelez wandb.log\(\) par défaut, cela incrémentera le compteur d’étapes.

**mode**

```python
 | @property
 | mode()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L455)

Pour compatibilité avec 0.9.x et antérieur, finira par être obsolète.

**group**

```python
 | @property
 | group()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L468)

 str : nom du groupe W&B associé avec l’essai.

Paramétrer un groupe aide l’IU de W&B à organiser les essais de manière logique.

Si vous faites de l’entraînement distribué, vous devriez attribuer tous les essais de cet entraînement au même groupe. Si vous faites de la validation croisée, vous devriez attribuer tous les blocs de validation croisée au même groupe.

**project**

```python
 | @property
 | project()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L487)

str : nom du projet W&B associé à l’essai.

**get\_url**

```python
 | get_url()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L491)

Renvoie : \(str, optionnel\) : URL de l’essai W&B, ou Aucun si l’essai est hors-ligne

**get\_project\_url**

```python
 | get_project_url()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L499)

Renvoie : \(str, optionnel\) : URL pour le projet W&B associé avec l’essai, ou Aucun si l’essai est hors-ligne

**get\_sweep\_url**

```python
 | get_sweep_url()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L507)

Renvoie : \(str, optionnel\) : URL pour le balayage associé avec l’essai, ou Aucun s’il n’y a pas de balayage associé ou si l’essai est hors-ligne.

**url**

```python
 | @property
 | url()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L516)

str : nom de l’URL W&B associée avec l’essai.

**entity**

```python
 | @property
 | entity()
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L521)

str : nom de l’entité W&B associée avec l’essai. L’entité est soit un nom d’utilisateur, soit un nom d’organisation.

**log**

```python
 | log(data, step=None, commit=None, sync=None)
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L675)

Enregistre un dict dans l’historique global de l’essai.

wandb.log peut être utilisé pour tout enregistrer, depuis des scalaires jusqu’aux histogrammes, au média et aux graphiques matplotlib.

L’utilisation la plus basique est wandb.log\({'train-loss': 0.5, 'accuracy': 0.9}\). Ceci sauvegardera une ligne d’historique associée avec l’essai qui comportera train-loss=0.5 et accuracy=0.9. Ces valeurs d’historique peuvent être tracées sur app.wandb.ai ou sur un serveur local. Les valeurs d’historique peuvent aussi être téléchargées par l’API wandb.

Enregistrer une valeur mettra à jour les valeurs du sommaire pour toute mesure enregistrée. Les valeurs du sommaire apparaîtront dans le tableau de l’essai sur app.wandb.ai ou sur un serveur local. Si une valeur de sommaire est paramétrée manuellement avec, par exemple, wandb.run.summary\["accuracy"\] = 0.9 , wandb.log ne mettra plus automatiquement à jour la précision \(accuracy\) de l’essai.

Les valeurs enregistrées n’ont pas besoin d’être des scalaires. L’enregistrement de tout objet wandb est pris en charge. Par exemple, wandb.log\({"example": wandb.Image\("myimage.jpg"\)}\) chargera une image d’exemple, qui sera joliment affichée dans l’IU wandb. Consultez [https://docs.wandb.com/library/reference/data\_types](https://docs.wandb.com/library/reference/data_types)  pour tous les différents formats pris en charge.

Il est recommandé d’enregistrer des mesures imbriquées, prises en charge par l’API wandb, pour pouvoir enregistrer de multiples valeurs avec wandb.log\({'dataset-1': {'acc': 0.9, 'loss': 0.3} ,'dataset-2': {'acc': 0.8, 'loss': 0.2}}\) et les mesures seront organisées dans l’IU wandb.

W&B garde une trace de l’étape globale, et enregistrer des mesures connexes ensemble est encouragé, c’est pourquoi par défaut à chaque fois que wandb.log est appelé, l’étape globale est incrémentée. Si ce n’est pas pratique d’enregistrer des mesures connexes ensemble, appeler wandb.log\({'train-loss': 0.5, commit=False}\) puis wandb.log\({'accuracy': 0.9}\) revient à appeler wandb.log\({'train-loss': 0.5, 'accuracy': 0.9}\)

wandb.log n’est pas censé être appelé plus de quelques fois par seconde. Si vous voulez enregistrer plus souvent que ce rythme, il est préférable d’agréger les données du côté client, ou vous pourriez avoir des performances dégradées.

**Arguments**:

* `row` _dict, optionnel – Un dictionnaire d’objets sérialisables python i.e str, ints, floats, Tensors, dicts, ou wandb.data\_types_
* `commit` _booléen, optionnel – Enregistre le dict de mesures au serveur wandb et incrémente l’étape. Si False, wandb.log met simplement à jour le dict de mesures actuel avec l’argument row et les mesures ne seront pas sauvegardées jusqu’à ce que wandb.log soit appelé avec commit=True._
* `step` _int, optionnel – L’étape globale de traitement. Ceci fait persister toutes les étapes antérieures qui n’ont pas été commit, mais par défaut, ne commit pas l’étape spécifiée._
* `sync` _booléen, True – Cet argument est obsolète et actuellement, ne change pas le comportement de wandb.log_

 **Exemples :**

 Utilisation basique

```text
- `wandb.log({'accuracy'` - 0.9, 'epoch': 5})
```

 Enregistrement incrémental

```text
- `wandb.log({'loss'` - 0.2}, commit=False)
# Somewhere else when I'm ready to report this step:
- `wandb.log({'accuracy'` - 0.8})
```

Histogramme

```text
- `wandb.log({"gradients"` - wandb.Histogram(numpy_array_or_sequence)})
```

Image

```text
- `wandb.log({"examples"` - [wandb.Image(numpy_array_or_pil, caption="Label")]})
```

Vidéo

```text
- `wandb.log({"video"` - wandb.Video(numpy_array_or_video_path, fps=4,
format="gif")})
```

Tracé Matplotlib

```text
- `wandb.log({"chart"` - plt})
```

 Courbe PR

```text
- `wandb.log({'pr'` - wandb.plots.precision_recall(y_test, y_probas, labels)})
```

 Objet 3D

```text
wandb.log({"generated_samples":
[wandb.Object3D(open("sample.obj")),
wandb.Object3D(open("sample.gltf")),
wandb.Object3D(open("sample.glb"))]})
```

Pour plus d’exemples, voir [https://docs.wandb.com/library/log](https://docs.wandb.com/library/log)

**Soulève :**

wandb.Error – si appelé avant wandb.init ValueError – si des données invalides sont passées

**save**

```python
 | save(glob_str: Optional[str] = None, base_path: Optional[str] = None, policy: str = "live")
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L810)

S’assure que tous les fichiers qui correspondent à glob\_str sont synchronisés à wandb avec spécification de policy.

**Arguments**:

* `glob_str` _string_ – un chemin relatif ou absolu vers un glob unix ou un chemin classique. Si ce n’est pas spécifié, la méthode est un noop.
* `base_path` _string – le chemin de base sur lequel exécuter le glob relatif à_
* `policy` _string_ – sur "live", "now", ou "end"
* `live` - télécharge le fichier pendant qu’il change, écrasant la version précédente
* `end` -ne télécharge le fichier qu’à la fin de l’essai

**finish**

```python
 | finish(exit_code=None)
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L906)

Marque un essai comme fini, et finit de télécharger toutes les données. C’est utilisé lorsque l’on crée plusieurs essais dans un même processus. Nous appelons automatiquement cette méthode lorsque votre script se ferme.

**join**

```python
 | join(exit_code=None)
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L920)

Alternative obsolète à finish\(\) – merci d’utiliser finish

**plot\_table**

```python
 | plot_table(vega_spec_name, data_table, fields, string_fields=None)
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L924)

Crée un graphique personnalisé dans un tableau.

**Arguments**:

* `vega_spec_name` - le nom de la spec pour ce graphique
* `table_key` - la clef utilisée pour enregistrer le tableau de données \(data table\)
* `data_table` - un objet wandb.Table qui contient les données qui vont être utilisées pour la visualisation
* `string_fields` - un dict qui fournit des valeurs pour toute chaîne de données constante nécessaire pour la visualisation personnalisée

**use\_artifact**

```python
 | use_artifact(artifact_or_name, type=None, aliases=None)
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L1566)

Déclare un artefact come input dans un essai, appelez `download` ou `file` sur l’objet retourné pour obtenir le contenu de manière locale.

**Arguments**:

* `artifact_or_name` _str ou Artifact_ – Un nom d’artefact. Peut avoir un préfixe d’entité/de projet. Les noms valides peuvent avoir la forme suivante : 

  name:version

  name:alias

  digest

  Vous pouvez aussi passer un objet Artifact créé en appelant `wandb.Artifact`

* `type` _str, optionnel – Le type d’artefact à utiliser._
* `aliases` _list, optionnel_ – Alias à appliquer à cet artefact.

 **Renvoie :**

Un objet `Artifact`.

**log\_artifact**

```python
 | log_artifact(artifact_or_path, name=None, type=None, aliases=None)
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L1621)

 Déclare un artefact comme output d’un essai.

**Arguments**:

* `artifact_or_path` _sstr ou Artifact_ – Un chemin jusqu’au contenu de cet artefact, peut être trouvé sous les formes suivantes :

  /local/répertoire

  /local/répertoire/fichier.txt

  s3://bucket/path

  Vous pouvez aussi passer un objet Artifact créé en appelant

  `wandb.Artifact`.

* `name` _str, optionnel_ – Un nom d’artefact. Peut avoir un préfixe avec entité/projet. Les noms valides peuvent avoir la forme suivante : 

  name:version

  name:alias

  digest

  Par défaut, sera le nom de base du chemin précédé du run id actuel s’il n’est pas spécifié.

* `type` _str_ - Le type d’artefact à enregistrer, par exemple "dataset", "model"
* `aliases` _list, optionnel_ – Alias à appliquer à cet artefact, par défaut \["latest"\] \(plus récents\)

 **Renvoie :**

Un objet `Artifact` 

**alert**

```python
 | alert(title, text, level=None, wait_duration=None)
```

[\[source\]](https://github.com/wandb/client/blob/025b586d2951e741c7fbac2df201b9836211b679/wandb/sdk/wandb_run.py#L1675)

Lance une alerte avec le titre \(title\) et le texte \(text\) donnés.

**Arguments**:

* `title` _str_ - Le titre de l’alerte, doit faire moins de 64 caractères de long
* `text` _str_ - Le corps de texte de l’alerte
* `level` _str ou wandb.AlertLevel, optionnel  - Le niveau d’alerte à utiliser :_ "INFO", "WARN", ou "ERROR"
* `wait_duration` _int, float, ou timedelta, optionnel_ – Le temps à attendre \(en secondes\) avant d’envoyer une autre alerte avec ce titre

