# Artifacts API

 Utilisez les Artefacts W&B pour garder une trace de vos datasets et faire du versioning du modèle. Initialisez un essai, créez un artefact, puis utilisez-le dans la partie suivante de votre flux de travail. Vous pouvez utiliser les artefacts pour garder une trace de vos fichiers et les sauvegarder, ou garder une trace des URI externes.

Cette fonctionnalité est disponible dans le client à partir de la version 0.9.0 de `wandb` 

## 1. Initialiser un essai

 Pour suivre une étape dans votre pipeline, initialisez un essai dans votre script. Spécifiez une chaîne de données pour **job\_type** pour différencier différentes étapes de pipeline – prétraitement \(preprocessing\), entraînement \(training\), évaluation \(evaluation\), etc. Si vous n’avez jamais instrumenté d’essai avec W&B, nous avons des guides plus détaillés sur le suivi d’expérience dans notre docu de [Librairie Python](https://docs.wandb.ai/library).

```python
run = wandb.init(job_type='train')
```

## 2. Créer un artefact

 Un artefact est comme un dossier de données, avec des contenus qui sont de vrais fichiers stockés dans l’artefact ou des références à des URI externes. Pour créer un artefact, enregistrez-le comme la sortie \(output\) d’un essai. Spécifiez une chaîne de données de **type** pour différencier les artefacts entre eux – dataset \(dataset\), modèle \(model\), résultat \(result\), etc. Donnez un **name** à cet artefact, comme `bike-dataset`, pour vous aider à vous rappeler ce que cet artefact contient. Dans une étape plus lointaine de votre pipeline, vous pouvez utiliser ce nom avec une version ajoutée dessus, comme `bike-dataset:v1 pour télécharger cet artefact.`

 Lorsque vous appelez **log\_artifact**, nous vérifions si les contenus de l’artefact ont changé, et si c’est le cas, nous créons automatiquement une nouvelle version de cet artefact : v0, v1, v2 etc.

**wandb.Artifact\(\)**

*    **type \(str\)** : Différencie les sortes d’artefacts, utilisé à des fins d’organisation. Nous vous recommandons de rester sur "dataset", "model" et "result".
* **name \(str\) :** Donne un nom unique à votre artefact, utilisé lorsque vous faites référence à votre artefact autre part. Vous pouvez utiliser des chiffres, des lettres, des tirets du bas et du haut, et des points dans le nom.
*   **description \(str, optionnel\) :** Texte libre affiché à côté de la version d’artefact dans l’IU
*   **metadata \(dict, optionnel\) :** Données structurées associées avec l’artefact, par exemple la distribution de classe d’un dataset. Grâce à la manière dont nous avons construit l’interface web, vous pourrez utiliser ces données pour faire des requêtes et tracer des graphiques.

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')

# Add a file to the artifact's contents
artifact.add_file('bicycle-data.h5')

# Save the artifact version to W&B and mark it as the output of this run
run.log_artifact(artifact)
```

{% hint style="warning" %}
**Note : Les appels pour** `log_artifact sont faits de manière asynchrone pour les téléchargements en cours vers le cloud. Cela peut créer un comportement surprenant lorsqu’on enregistre des artefacts dans une boucle. Par exemple :`

```text
for i in range(10):
    a = wandb.Artifact('race', type='dataset', metadata={
        "index": i,
    })
    # ... add files to artifact a ...
    run.log_artifact(a)
```

 La version d’artefact **v0** n’est pas garantie d’avoir un index de 0 dans ses métadonnées, puisque les artefacts peuvent être enregistrés dans un ordre aléatoire.
{% endhint %}

## 3. Utiliser un artefact

Vous pouvez utiliser un artefact comme entrée \(input\) d’un essai. Par exemple, nous pourrions prendre `bike-dataset:v0` , la première version de `bike-dataset`, et l’utiliser dans le prochain script de notre pipeline. Lorsque vous appelez **use\_artefact**, votre script W&B envoie une requête pour trouver cet artefact nommé et le marquer comme entrée à l’essai.

```python
# Query W&B for an artifact and mark it as input to this run
artifact = run.use_artifact('bike-dataset:v0')

# Download the artifact's contents
artifact_dir = artifact.download()
```

**Utiliser un artefact depuis un projet différent**  
Vous pouvez librement faire référence à vos artefacts depuis n’importe quel projet auquel vous avez accès en qualifiant le nom de l’artefact avec son nom de projet. Vous pouvez aussi faire référence à des artefacts à travers des entités en qualifiant encore plus le nom de l’artefact avec son nom d’entité.

```python
# Query W&B for an artifact from another project and mark it
# as an input to this run.
artifact = run.use_artifact('my-project/bike-model:v0')

# Use an artifact from another entity and mark it as an input
# to this run.
artifact = run.use_artifact('my-entity/my-project/bike-model:v0')
```

**Utiliser un artefact qui n’a pas été enregistré**  
Vous pouvez aussi construire un objet artefact et le passer dans **use\_artifact**. Nous vérifions si cet artefact existe déjà dans W&B, et si ce n’est pas le cas, nous créons un nouvel artefact. Ceci est idempotent – vous pouvez passer un artefact dans use\_artifact autant de fois que vous le souhaitez, et nous le dédupliquerons tant que son contenu restera le même.

```python
artifact = wandb.Artifact('bike-model', type='model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

## Versions et alias

Lorsque vous enregistrez un artefact pour la première fois, nous créons la version **v0**. Lorsque vous enregistrez de nouveau ce même artefact, nous faisons une somme de contrôle, et si l’artefact a changé, nous sauvegardons une nouvelle version, **v1**.

Vous pouvez utiliser les alias comme des pointeurs vers des versions spécifiques. Par défaut, run.log\_artifact ajoute l’alias **le plus récent** \(latest\) à la version enregistrée.

Vous pouvez aller chercher un artefact en utilisant un alias. Par exemple, si vous voulez que votre script d’entraînement tire toujours la version la plus récente d’un dataset, spécifiez **latest** lorsque vous utilisez cet artefact.

```python
artifact = run.use_artifact('bike-dataset:latest')
```

 Vous pouvez aussi utiliser un alias personnalisé pour une version d’artefact. Par exemple, si vous voulez marquer lequel de vos checkpoints de modèle est le meilleur pour la mesure AP-50, vous pouvez ajouter la chaîne **best-ap50** en tant qu’alias lorsque vous enregistrez l’artefact de modèle.

```python
artifact = wandb.Artifact('run-3nq3ctyy-bike-model', type='model')
artifact.add_file('model.h5')
run.log_artifact(artifact, aliases=['latest','best-ap50'])
```

##  Construire des artefacts

Un artefact est comme un dossier de données. Chaque entrée est soit un vrai fichier qui est stocké dans l’artefact, soit une référence à un URI externe. Vous pouvez imbriquer des dossiers à l’intérieur de l’artefact, comme dans un système de fichier classique. Construisez de nouveaux artefacts en initialisant la classe `wandb.Artifact()` class.

Vous pouvez passer les champs suivants dans un constructeur `Artifact()`, ou paramétrez-les directement sur un objet artefact

* **type:** devrait être ‘dataset’, ‘model’, ou ‘result’
* **description**: texte libre qui sera affiché dans l’IU.
* **metadata**:Un dictionnaire qui peut contenir tout type de données structurées. Vous serez capable d’utiliser ces données pour faire des requêtes et tracer des graphiques. E.g vous pourriez choisir de stocker la distribution de classe pour l’artefact de dataset comme métadonnées \(metadata\).

```python
artifact = wandb.Artifact('bike-dataset', type='dataset')
```

Utilisez **name** pour spécifier le nom d’un fichier optionnel, ou un préfixe de chemin de fichier si vous ajoutez un répertoire.

```python
# Add a single file
artifact.add_file(path, name='optional-name')

# Recursively add a directory
artifact.add_dir(path, name='optional-prefix')

# Return a writeable file-like object, stored as <name> in the artifact
with artifact.new_file(name) as f:
    ...  # Write contents into the file 

# Add a URI reference
artifact.add_reference(uri, name='optional-name')
```

### Ajouter des fichiers et des répertoires

Pour les exemples suivants, supposons que nous avons un répertoire de projet avec ces fichiers :

```text
project-directory
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Resulting artifact contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;checkpoints/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_file(&apos;model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_dir(&apos;images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.new_file(&apos;hello.txt&apos;)</td>
      <td style="text-align:left">hello.txt</td>
    </tr>
  </tbody>
</table>

### Ajouter des références

```python
artifact.add_reference(uri, name=None, checksum=True)
```

* **uri \(string\):**  L’URI de référence à suivre.
* **name \(string\):** Un écrasement optionnel du nom. S’il n’est pas fourni, un nom sera inféré depuis **uri**.
* **checksum \(bool\):** Si True, la référence récolte les informations de somme de contrôles et les métadonnées depuis **uri** dans un but de validation.

Vous pouvez ajouter des références à des URI externes à des artefacts, plutôt que de vrais fichiers. Si un URI possède un schéma que wandb sait gérer, l’artefact gardera une trace des sommes de contrôle et d’autres informations dans un but de reproductibilité. Les artefacts prennent actuellement en charge les schémas URI suivants :

* `http(s)://`: Un fichier à un fichier accessible par http. Cet artefact gardera une trace des sommes de contrôles sous la forme d’etags et de métadonnées de taille si le serveur http prend en charge les en-têtes de réponses \(response headers\) `ETag` et `Content-Length.`
* `s3://`: Un chemin vers un objet ou un préfixe d’objet dans S3. Cet artefact gardera une trace des sommes de contrôle et des informations de versioning \(si le versioning objet du bucket est activé\) pour les objets référencés. Les préfixes d’objets sont agrandis pour inclure les objets sous le préfixe, jusqu’à un maximum de 10 000 objets.
* `gs://`: Un chemin vers un objet ou un préfixe d’objet dans GCS. L’artefact gardera une trace des sommes de contrôles et des informations de versioning \(si le versioning objet du bucket est activé\) pour les objets référencés. Les préfixes d’objets sont agrandis pour inclure les objets sous le préfixe, jusqu’à un maximum de 10 000 objets..

Pour les exemples suivants, supposons que nous avons un bucket S3 avec ces fichiers :

```text
s3://my-bucket
|-- images
|   |-- cat.png
|   +-- dog.png
|-- checkpoints
|   +-- model.h5
+-- model.h5
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">API call</th>
      <th style="text-align:left"><b>Contenus artefacts r&#xE9;sultants</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;)</td>
      <td style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/checkpoints/model.h5&apos;)</td>
      <td
      style="text-align:left">model.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/model.h5&apos;, name=&apos;models/mymodel.h5&apos;)</td>
      <td
      style="text-align:left">models/mymodel.h5</td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;)</td>
      <td style="text-align:left">
        <p>cat.png</p>
        <p>dog.png</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">artifact.add_reference(&apos;s3://my-bucket/images&apos;, name=&apos;images&apos;)</td>
      <td
      style="text-align:left">
        <p>images/cat.png</p>
        <p>images/dog.png</p>
        </td>
    </tr>
  </tbody>
</table>

## Utiliser et télécharger les artefacts

```python
run.use_artifact(artifact=None)
```

* Marque un artefact comme entrée \(input\) de votre essai.

Il y a deux schémas pour utiliser les artefacts. Vous pouvez utiliser un nom d’artefact qui est explicitement stocké dans W&B, ou vous pouvez construire un objet artefact et le passer pour qu’il soit dédupliqué si nécessaire.

###  Utiliser un artefact stocké dans W&B

```python
artifact = run.use_artifact('bike-dataset:latest')
```

 Vous pouvez appeler les méthodes suivantes sur l’artefact renvoyé :

```python
datadir = artifact.download(root=None)
```

* Télécharge tous les contenus de l’artefact qui ne sont pas présents pour l’instant. Cela renvoie un chemin vers un répertoire qui contient les contenus de l’artefact. Vous pouvez explicitement spécifier la destination de téléchargement en paramétrant **root**.

```python
path = artifact.get_path(name)
```

*   Ne ramène que le fichier au `name` du chemin. Renvoie un objet `Entry` avec les méthodes suivantes :
  * **Entry.download\(\)**: Télécharge le fichier depuis l’artefact au `name` du chemin
  * **Entry.ref\(\)**: Si cette entrée a été stockée sous forme de référence en utilisant `add_reference`, renvoie l’URI

 Les références qui ont des schémas que W&B sait prendre en charge peuvent être téléchargées comme des fichiers artefacts. L’API du consommateur est la même.

###  Construire et utiliser un artefact

Vous pouvez aussi construire un objet artefact et le passer dans **use\_artifact**. Cela créera un artefact dans W&B s’il n’existe pas encore. Ceci est idempotent, vous pouvez donc le faire autant de fois que vous le souhaitez. Cet artefact ne sera créé qu’une seule fois, tant que les contenus de `model.h5 restent les mêmes.`

```python
artifact = wandb.Artifact('reference model')
artifact.add_file('model.h5')
run.use_artifact(artifact)
```

### Télécharger un artefact en dehors d’un essai

```python
api = wandb.Api()
artifact = api.artifact('entity/project/artifact:alias')
artifact.download()
```

##  Mettre les artefacts à jour

Vous pouvez mettre à jour la `description (description)`, les `métadonnées (metadata)` et les `alias (aliases)` d’un artefact en les paramétrant simplement sur les valeurs désirées puis en appelant `save()`.

```python
api = wandb.Api()
artifact = api.artifact('bike-dataset:latest')

# Update the description
artifact.description = "My new description"

# Selectively update metadata keys
artifact.metadata["oldKey"] = "new value"

# Replace the metadata entirely
artifact.metadata = {"newKey": "new value"}

# Add an alias
artifact.aliases.append('best')

# Remove an alias
artifact.aliases.remove('latest')

# Completely replace the aliases
artifact.aliases = ['replaced']

# Persist all artifact modifications
artifact.save()
```

## Parcourir le graphique artefact

W&B garde automatiquement une trace des artefacts qu’un essai donné a enregistré ainsi que des artefacts qu’un essai donné a utilisés. Vous pouvez parcourir ce graphique en utilisant les APIs suivantes :

* `artifact.logged_by()` Parcourir de haut en bas le graphique depuis un artefact :
* `run.used_artifacts()` Parcourir de haut en bas le graphique depuis un essai :

```python
artifact = wandb.use_artifact('data:v0')
producer_run = artifact.logged_by()
input_artifacts = producer_run.used_artifacts()
```

## Confidentialité des données

Les artefacts utilisent un contrôle d’accès sécurisé par API. Les fichiers sont cryptés lorsqu’ils sont stockés et en transit. Les artefacts peuvent aussi garder une trace des références vers des buckets privés sans envoyer de contenu de fichiers à W&B. Pour d’autres alternatives, contactez-nous sur [contact@wandb.com](mailto:contact@wandb.com) pour parler de cloud privé et d’installations sur site. 

