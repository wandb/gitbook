# Databricks

 W&B s’intègre avec [Databricks](https://www.databricks.com/) en personnalisant l’expérience W&B Jupyter notebook dans l’environnement Databricks.

## Configuration Databricks

### Installer wandb dans le cluster

Naviguez dans la configuration de votre cluster, choisissez votre cluster, cliquez sur Libraries, puis sur Install New, Choose PyPI et ajoutez le package `wandb`.

### Authentification

Pour authentifier votre compte W&B, vous pouvez ajouter un secret databricks que vos notebooks peuvent quérir.

```bash
# install databricks cli
pip install databricks-cli

# Generate a token from databricks UI
databricks configure --token

# Create a scope with one of the two commands (depending if you have security features enabled on databricks):
# with security add-on
databricks secrets create-scope --scope wandb
# without security add-on
databricks secrets create-scope --scope wandb --initial-manage-principal users

# Add your api_key from: https://app.wandb.ai/authorize
databricks secrets put --scope wandb --key api_key
```

## Exemples

###  Simple

```python
import os
import wandb

api_key = dbutils.secrets.get("wandb", "api_key")
wandb.login(key=api_key)

wandb.init()
wandb.log({"foo": 1})
```

### Balayages

Set-up \(temporairement\) nécessaire pour les notebooks qui essayent d’utiliser wandb.sweep\(\) ou wandb.agent\(\) :

```python
import os
# These will not be necessary in the future
os.environ['WANDB_ENTITY'] = "my-entity"
os.environ['WANDB_PROJECT'] = "my-project-that-exists"
```

Nous expliquons plus en détails comment effectuer un balayage dans un notebook ici :

{% page-ref page="../sweeps/python-api.md" %}

