# Settings

## wandb.sdk.wandb\_settings

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L2)

Ce module configure les paramètres pour les runs wandb.

Ordre de chargement des paramètres : \(diffère de la priorité\) defaults environment wandb.setup\(settings=\) system\_config workspace\_config wandb.init\(settings=\) network\_org network\_entity network\_project

Priorité des paramètres : Voir variable "source"

Lorsque l’override est utilisé, il a la priorité sur des paramètres qui sont non-override.

Les priorités override sont dans l’ordre inverse des priorités non-override.

###  Objets Settings

```python
class Settings(object)
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L187)

Constructeur de Paramètres

**Arguments**:

* `entity` - utilisateur personnel ou équipe à utiliser pour ce Run.
* `project` - nom du projet pour ce Run.

**Soulève**

* `Exception` - si problème.

**\_\_copy\_\_**

```python
 | __copy__()
```

 [\[voir\_source\]](https://github.com/wandb/client/blob/1d91d968ba0274736fc232dcb1a87a878142891d/wandb/sdk/wandb_settings.py#L656)

Copie \(notez que l’objet copié ne sera pas figé\).

