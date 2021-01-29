# Data Import/Export API

Exporte une dataframe pour analyse personnalisée, ou ajoute des données de manière asynchrone à un essai complété. Pour plus de détails, voir la [Référence API](https://docs.wandb.ai/ref/export-api/api).

### Authentication

Authentifiez votre machine avec votre [clef API](https://wandb.ai/authorize) d’une de ces deux manières :

1.  Exécutez `wandb login` sur votre ligne de command et copiez votre clef API.
2. Paramètre la variable d’environnement **WANDB\_API\_KEY** sur votre clef API.

### Exporter des données d’essai

 Téléchargez des données d’un essai actif ou complété. Une utilisation courante inclut de télécharger une dataframe pour une analyse personnalisée dans un Jupyter notebook, ou utiliser une logique personnalisée dans un environnement automatisé.

```python
import wandb
api = wandb.Api()
run = api.run("<entity>/<project>/<run_id>")
```

Les attributs les plus utilisés pour un objet run sont :

| Attribut | Signification |
| :--- | :--- |
| run.config | Un dictionnaire pour les inputs de modèle, comme les hyperparamètres |
| run.history\(\) | Une liste de dictionnaires destinés à stocker des valeurs qui changent pendant l’entraînement du modèle, comme les pertes. La commande wandb.log\(\) est annexé à cet objet. |
| run.summary | Un dictionnaire d’outputs. Ce peut être des scalaires comme la précision et la perte, ou de grands fichiers. Par défaut, wandb.log\(\) paramètre le sommaire sur la valeur finale d’une série chronologique enregistrée. Peut aussi être paramétré directement. |

Vous pouvez aussi modifier ou mettre à jour les données d’essais précédents. Par défaut, une seule instance d’un objet api mettra en cache toutes les requêtes de réseaux. Si votre cas d’utilisation requiert des informations en temps réel dans un script en cours d’exécution, appelez api.flush\(\) pour obtenir les valeurs mises à jour.

###  Échantillonnage

La méthode d’historique par défaut échantillonne les mesures pour un nombre fixé d’échantillons \(par défaut, 500. Vous pouvez modifier ceci avec l’argument samples\). Si vous voulez exporter toutes les données d’un essai conséquent, vous pouvez utiliser la méthode run.scan\_history\(\). Pour plus de détails, voir la [Référence API](https://docs.wandb.ai/ref/export-api/api). 

### Requérir plusieurs essais

{% tabs %}
{% tab title="MongoDB Style" %}
L’API W&B fournit également un moyen de requérir plusieurs essais dans un projet avec api.runs\(\). Le cas d’utilisation le plus courant est l’export de données d’essais pour analyses personnalisées. L’interface de requête est la même que celle [utilisée par MongoDB](https://docs.mongodb.com/manual/reference/operator/query).

```python
runs = api.runs("username/project", {"$or": [{"config.experiment_name": "foo"}, {"config.experiment_name": "bar"}]})
print("Found %i" % len(runs))
```
{% endtab %}

{% tab title="Dataframes et CSV" %}
Cet exemple de script trouve un projet et output un CSV d’essais avec nom, configurations, et statistiques de sommaire.

```python
import wandb
api = wandb.Api()

# Change oreilly-class/cifar to <entity/project-name>
runs = api.runs("<entity>/<project>")
summary_list = [] 
config_list = [] 
name_list = [] 
for run in runs: 
    # run.summary are the output key/values like accuracy.  We call ._json_dict to omit large files 
    summary_list.append(run.summary._json_dict) 

    # run.config is the input metrics.  We remove special values that start with _.
    config_list.append({k:v for k,v in run.config.items() if not k.startswith('_')}) 

    # run.name is the name of the run.
    name_list.append(run.name)       

import pandas as pd 
summary_df = pd.DataFrame.from_records(summary_list) 
config_df = pd.DataFrame.from_records(config_list) 
name_df = pd.DataFrame({'name': name_list}) 
all_df = pd.concat([name_df, config_df,summary_df], axis=1)

all_df.to_csv("project.csv")
```
{% endtab %}
{% endtabs %}

Appeler `api.runs(...)` renvoie un objet **Runs** qui est itératif et agit comme une liste. L’objet charge 50 essais \(runs\) d’un coup en séquence comme requis, vous pouvez changer le nombre d’essais chargés par page avec l’argument mot-clef **per\_page**.

`api.runs(...)` accepte aussi un argument mot-clef **order**. L’ordre par défaut est `-created_at`, spécifiez `-created_at`pour obtenir des résultats par ordre croissant. Vous pouvez aussi trier par config ou valeur de sommaire, i.e. `summary.val_acc` ou `config.experiment_name`

### Gestion d’erreur

 Si des erreurs surviennent en communiquant avec les serveurs W&B, une erreur `wandb.CommError sera soulevée. L’exception originale peut être retrouvée via l’attribut` **`exc`**`.`

### Obtenir le dernier git commit par l’API

 Dans l’IU, cliquez sur un run puis sur l’onglet Vue d’ensemble sur la page de run pour voir le dernier git commit. Il est aussi présent dans le fichier `wandb-metadata.json` . En utilisant l’API publique, vous pouvez aussi obtenir le git hash avec **run.commit**.

## Questions fréquentes

### Exporter les données pour visualisation dans matplotlib ou seaborn

 Vous pouvez regarder nos [exemples API](https://docs.wandb.ai/ref/export-api/examples) pour avoir quelques schémas d’exports fréquents. Vous pouvez aussi cliquer sur le bouton télécharger sur un graphique personnalisé ou sur le tableau étendu de vos essais pour télécharger un CSV depuis votre navigateur.

### Obtenir l’ID d’essai aléatoire et le nom d’essai depuis votre script

 Après avoir appelé `wandb.init()` vous pouvez accéder à l’ID d’essai aléatoire ou le nom d’essai humainement lisible depuis votre script, comme ceci :

* ID de run unique \(8 caractères de long\) : `wandb.run.id`
* Nom d’essai aléatoire \(humainement lisible\) : `wandb.run.name`

Si vous vous demandez comment paramétrer des identificateurs utiles pour vos essais, voici ce que nous vous recommandons :

* **Run ID**: Laissez le nombre généré tel quel. Cette ID doit être unique à travers tous les essais de votre projet.
* **Run name**: **Doit être court, lisible, et de préférence unique pour que vous puissiez dire la différence entre les différentes lignes sur vos graphiques.**
* **Run notes**:  C’est un endroit parfait pour mettre une description rapide de ce que vous faites dans votre essai. Vous pouvez les paramétrer avec `wandb.init(notes="your notes here")` 
* **Run tags**: Gardez une trace dynamique des événements avec les étiquettes d’essai, et utilisez vos filtres dans l’IU pour épurer vos tableaux et ne voir que les essais qui vous intéressent. Vous pouvez paramétrer les étiquettes \(tags\) depuis votre script puis les éditer dans l’IU, à la fois sur le tableau des runs et sur l’onglet de vue d’ensemble de la page de run.

