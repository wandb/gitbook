---
description: >-
  Syntaxe pour paramétrer les plages d’hyperparamètres, la stratégie de
  recherche, et d’autres aspects de vos balayages
---

# Configuration

Utilisez ces champs de configuration pour personnaliser votre balayage. Il y a deux manières de spécifier votre configuration :

1.  [Fichier YAML ](https://docs.wandb.com/sweeps/quickstart#2-sweep-config): idéal pour les balayages distribués. Voir des exemples [ici](https://github.com/wandb/examples/tree/master/examples/keras/keras-cnn-fashion).
2.  [Structure de données Python ](https://docs.wandb.ai/sweeps/python-api): idéal pour exécuter un balayage dans un Jupyter Notebook

| **Clef de niveau supérieur** | **Signification** |
| :--- | :--- |
| name | Le nom du balayage, affiché dans l’IU W&B |
| description | Description textuelle du balayages \(notes\) |
| program | Script d’entraînement à exécuter \(requis\) |
| metric | Spécifie la mesure à optimiser \(utilisé dans certaines stratégies de recherches et de critères d’arrêt\) |
| method | Spécifie la [stratégie de recherche](https://docs.wandb.ai/sweeps/configuration#search-strategy) \(requis\) |
| early\_terminate | Spécifie les critères d’arrêts \(optionnel, par défaut, aucun arrêt précoce\) |
| parameters | Spécifie les[ paramètres](https://docs.wandb.ai/sweeps/configuration#parameters) liés à la recherche \(requis\) |
| project | Spécifie le projet pour ce balayage |
| entity | Spécifie l’entité pour ce balayage |
| command | Spécifie [la ligne de commande](https://docs.wandb.ai/sweeps/configuration#command) de référence d’exécution du script d’entraînement |

### Metric

Spécifiez la mesure à optimiser. Cette mesure doit être explicitement enregistrée dans W&B par votre script d’entraînement. Par exemple, si vous voulez minimiser la perte de validation \(validation loss\) de votre modèle :

```python
# [model training code that returns validation loss as valid_loss]
wandb.log({"val_loss" : valid_loss})
```

| `metric` sub-key | **Signification** |
| :--- | :--- |
| name | Nom de la mesure à optimiser |
| goal | `minimize` ou `maximize` \(Default is `minimize`\) |
| target | Valeur cible pour la mesure que vous optimisez. Lorsqu’un essai dans un balayage parvient à cette valeur cible, l’état du balayage est réglé sur **finished** \(terminé\). Cela signifie que tous les agents avec des essais actifs finiront leur tâche en cours, mais qu’aucun nouvel essai ne sera lancer dans ce balayage. |

 ⚠️ Impossible d’optimiser les mesures imbriquées

La mesure que vous optimisez doit être au **niveau supérieur** de la config.

Ceci ne fonctionnera **PAS** :  
Configuration de balayage  
`metric:   
    name: my_metric.nested`   
_Code_  
`nested_metrics = {"nested": 4} wandb.log({"my_metric", nested_metrics}`

Solution : enregistrer la mesure au niveau supérieur

Configuration de balayage  
`metric:   
    name: my_metric_nested`   
_Code_`nested_metrics = {"nested": 4} wandb.log{{"my_metric", nested_metric} wandb.log({"my_metric_nested", nested_metric["nested"]})`



 **Exemples**

{% tabs %}
{% tab title="Maximiser " %}
```text
metric:
  name: val_loss
  goal: maximize
```
{% endtab %}

{% tab title="Minimiser" %}
```text
metric:
  name: val_loss
```
{% endtab %}

{% tab title="Cible" %}
```text
metric:
  name: val_loss
  goal: maximize
  target: 0.1
```
{% endtab %}
{% endtabs %}

###  Stratégie de recherche

Spécifiez la stratégie de recherche avec la clef `method` dans la configuration de balayage.

| `method` | **Signification** |
| :--- | :--- |
| grid | La recherche de grille fait des itérations sur toutes les combinaisons possibles de valeurs de paramètres. |
| random | La recherche aléatoire choisit des sets aléatoires de valeurs. |
| bayes | L’Optimisation Bayésienne utilise un processus gaussien pour modéliser la fonction puis pour choisir les paramètres pour optimiser la probabilité d’amélioration. Cette stratégie requiert qu’une clef de mesure soit spécifiée. |

**Exemples**

{% tabs %}
{% tab title="Recherche aléatoire" %}
```text
method: random
```
{% endtab %}

{% tab title="Recherche de grille" %}
```text
method: grid
```
{% endtab %}

{% tab title="Recherche bayésienne " %}
```text
method: bayes
metric:
  name: val_loss
  goal: minimize
```
{% endtab %}
{% endtabs %}

### Critères d’arrêt

L’arrêt précoce est une fonctionnalité optionnelle qui accélère la recherche d’hyperparamètres en coupant court aux essais qui ne sont pas prometteurs. Lorsque l’arrêt précoce est déclenché, l’agent arrête l’essai en cours et obtient le prochain set d’hyperparamètres à essayer.

| `early_terminate` sub-key | Meaning |
| :--- | :--- |
| type | specify the stopping algorithm |

We support the following stopping algorithm\(s\):

| `type` | **Signification** |
| :--- | :--- |
| hyperband |  spécifie[ l’algorithme d’arrêt](https://arxiv.org/abs/1603.06560) |

 L’arrêt Hyperband évalue si un programme devrait être arrêté ou s’il lui est permis de continuer à une ou plusieurs parenthèses \(brackets\) durant l’exécution du programme. Les parenthèses sont configurées pour être des itérations statiques pour une `mesure ( metric )` spécifiée \( où une itération est le nombre de fois qu’une mesure a été enregistrée – si la mesure est enregistrée à chaque epoch, ce sont alors des itérations d’epoch\).

Pour spécifier la planification des parenthèses, il faut que `min_iter` or `max_iter soit définie.`

| `early_terminate` sub-key | Meaning |
| :--- | :--- |
| min\_iter | spécifie l’itération pour la première parenthèse |
| max\_iter | spécifie le nombre maximal d’itérations pour le programme |
| s | spécifie le nombre total de parenthèses \(requis pour `max_iter`\) |
| eta | spécifie la planification de multiplication des parenthèses \(par défaut : 3\) |

 **Exemples**

{% tabs %}
{% tab title="Hyperband \(min\_iter\)" %}
```text
early_terminate:
  type: hyperband
  min_iter: 3
```

Brackets: 3, 9 \(3\*eta\), 27 \(9 \* eta\), 81 \(27 \* eta\)
{% endtab %}

{% tab title="Hyperband \(max\_iter\)" %}
```text
early_terminate:
  type: hyperband
  max_iter: 27
  s: 2
```

Brackets: 9 \(27/eta\), 3 \(9/eta\)
{% endtab %}
{% endtabs %}

### Paramètres

 Décrivez les hyperparamètres à explorer. Pour chaque hyperparamètre, spécifiez le nom et les valeurs possibles sous liste de constantes \(pour toute méthode\) ou spécifiez une distribution \(pour `random` ou `bayes` \).

| Values | **Signification** |
| :--- | :--- |
| values: \[\(type1\), \(type2\), ...\] | Spécifie toutes les valeurs valides pour cet hyperparamètre. Compatible avec`grid`. |
| value: \(type\) | Spécifie la valeur unique valide pour cet hyperparamètre. Compatible avec `grid`. |
| distribution: \(distribution\) | Sélectionne une distribution du tableau de distribution plus bas. Si non spécifié, sera par défaut `categorical` si `values est réglé, int_uniform` si `max` et `min sont réglées sur des entiers relatifs,` `uniform` si `max` et `min sont réglées sur des floats, ouconstant` si `value` est réglée. |
| min: \(float\) max: \(float\) | Valeurs valides maximum et `minimum` pour. |
| min: \(int\) max: \(int\) | Valeurs valides maximum et minimum pour les hyperparamètres distribués par `int_uniform` |
| mu: \(float\) | Paramètre moyen pour les hyperparamètres distribués par `normal` – ou `lognormal` |
| sigma: \(float\) | Paramètre de déviation standard pour les hyperparamètres distribués par `normal` – ou `lognormal` |
| q: \(float\) | Taille d’étape de quantification pour les hyperparamètres quantifiés |

**Exemple**

{% tabs %}
{% tab title="grid - single value" %}
```text
parameter_name:
  value: 1.618
```
{% endtab %}

{% tab title="grid - multiple values" %}
```text
parameter_name:
  values:
  - 8
  - 6
  - 7
  - 5
  - 3
  - 0
  - 9
```
{% endtab %}

{% tab title="random or bayes - normal distribution" %}
```text
parameter_name:
  distribution: normal
  mu: 100
  sigma: 10
```
{% endtab %}
{% endtabs %}

### Distributions

| Name | Meaning |
| :--- | :--- |
| constant |  Distribution constante. Doit spécifier `value`. |
| categorical | Distribution catégorielle. Doit spécifier `values`. |
| int\_uniform |  Distribution uniforme discrète sur des entiers relatifs. Doit spécifier `max` et`min` comme entiers relatifs. |
| uniform | Distribution uniforme continue. Doit spécifier `max` and `min` comme floats. |
| q\_uniform | Distribution uniforme quantifiée. Renvoie `round(X / q) * q où X est uniforme. Par défaut, q est` `1`. |
| log\_uniform | Distribution uniforme logarithmique. Renvoie une valeur entre `exp(min)` et `exp(max) de sorte que le logarithme naturel soit uniformément distribué entre min` et `max`. |
| q\_log\_uniform | Distribution uniforme logarithmique quantifiée. Renvoie `round(X / q) * q où X est` log\_uniform`. Par défaut, q est` `1`. |
| normal | Normal distribution. Return value is normally-distributed with mean `mu` \(default `0`\) and standard deviation `sigma` \(default `1`\). |
| q\_normal | Distribution normale. La valeur renvoyée est distribuée normalement avec une moyenne `mu` \(par défaut, `0`\) et une déviation standard `sigma` \(par défaut, `1`\). |
| log\_normal | Distribution normale logarithmique. Renvoie une valeur X de sorte que le logarithme naturel log\(X\) est normalement distribué avec une moyenne `mu`\(par défaut, `0`\) et une déviation standard `sigma` \(par défaut, `1`\). |
| q\_log\_normal | Distribution normale logarithmique quantifiée. Renvoie `round(X / q) * q où X est log_normal. Par défaut, q est` `1`. |

**Exemple**

{% tabs %}
{% tab title="constant" %}
```text
parameter_name:
  distribution: constant
  value: 2.71828
```
{% endtab %}

{% tab title="categorical" %}
```text
parameter_name:
  distribution: categorical
  values:
  - elu
  - celu
  - gelu
  - selu
  - relu
  - prelu
  - lrelu
  - rrelu
  - relu6
```
{% endtab %}

{% tab title="uniform" %}
```text
parameter_name:
  distribution: uniform
  min: 0
  max: 1
```
{% endtab %}

{% tab title="q\_uniform" %}
```text
parameter_name:
  distribution: q_uniform
  min: 0
  max: 256
  q: 1
```
{% endtab %}
{% endtabs %}

### Command Line Ligne de commande <a id="command"></a>

L’agent de balayage construit une ligne de commande avec le format suivant par défaut :

```text
/usr/bin/env python train.py --param1=value1 --param2=value2
```

{% hint style="info" %}
Sur les machines Windows, le /usr/bin/env sera omis. Sur les systèmes UNIX, il s’assure que le bon interprète python est choisi en se basant sur l’environnement.
{% endhint %}

Cette ligne de commande peut être modifiée en spécifiant une clef `command` dans le fichier de configuration.

Par défaut, cette commande est définie comme :

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args}
```

| Command Macro | Expansion |
| :--- | :--- |
| ${env} | /usr/bin/env sur les systèmes UNIX, omis sur Windows |
| ${interpreter\| | Se développe en "python". |
| ${program} | Script d’entraînement spécifié par la clef `program`  de configuration de balayage |
| ${args} | Arguments développés sous la forme --param1=value1 --param2=value2 |
| ${args\_no\_hyphens} | Arguments développés sous la forme param1=value1 param2=value2 |
| ${json} | Arguments encodés en JSON |
| ${json\_file} | Le chemin à un fichier qui contient les arguments encodés en JSON |

Examples:

{% tabs %}
{% tab title="Utiliser avec Hydra " %}
Vous pouvez changer la commande pour passer des arguments à la manière dont des outils comme Hydra s’y attendent.

```text
command:
  - ${env}
  - python3
  - ${program}
  - ${args}
```
{% endtab %}

{% tab title="Add extra parameters" %}
Si votre programme n’utilise pas de parsing d’argument, vous pouvez totalement éviter de passer vos arguments et tirer avantage du fait que `wandb.init() récolte automatiquement les paramètres de balayage :`

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - "-config"
  - your-training-config
  - ${args}
```
{% endtab %}

{% tab title="Ajouter des paramètres supplémentaires " %}
 Ajouter des arguments supplémentaires à la ligne de commande, qui ne sont pas spécifiés par les paramètres de configuration du balayage :

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
```
{% endtab %}

{% tab title="Utiliser avec Hydra" %}
Vous pouvez changer la commande pour passer des arguments à la manière dont des outils comme Hydra s’y attendent.

```text
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args_no_hyphens}
```
{% endtab %}
{% endtabs %}

##  Questions fréquentes

###  Config imbriquée

Pour l’instant, les balayages ne prennent pas en charge les valeurs imbriquées, mais nous prévoyons de les prendre en charge dans un avenir proche.

