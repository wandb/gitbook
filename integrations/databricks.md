# Databricks

W&B se integra con [Databricks](https://www.databricks.com/) al personalizar la experiencia de la notebook de Jupyter con W&B en el entorno de Databricks.

## Configuración de Databricks

### Instala wandb en el cluster

 Navega a la configuración de tu cluster, elige tu cluster, haz click en Libraries, entonces en Install New, Choose PyPI, y agrega el paquete `wandb`.

###  Autenticación

Para autenticar tu cuenta de W&B, puedes agregar un secreto de databricks, que tus notebooks pueden consultar.

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

##  Ejemplos

### Simple

```python
import os
import wandb

api_key = dbutils.secrets.get("wandb", "api_key")
wandb.login(key=api_key)

wandb.init()
wandb.log({"foo": 1})
```

###  Barridos

Ajuste requerido \(temporalmente\) para las notebooks intentando usar wandb.sweep\(\) o wandb.agent\(\):

```python
import os
# These will not be necessary in the future
os.environ['WANDB_ENTITY'] = "my-entity"
os.environ['WANDB_PROJECT'] = "my-project-that-exists"
```

Cubrimos más detalles de cómo ejecutar un barrido en una noteboob aquí:

{% page-ref page="../sweeps/python-api.md" %}

