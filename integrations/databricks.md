# Databricks

W＆Bは、[Databricks](https://www.databricks.com/)環境でW＆B Jupyterノートブックエクスペリエンスをカスタマイズすることにより、Databricksと統合します。

## Databricksの構成

###  **クラスターにwandbをインストールします**

クラスター構成に移動し、クラスターを選択し、\[ライブラリ\]、\[新規インストール\]の順にクリックし、\[PyPI\]を選択して、パッケージ`wandb`を追加します。

###  認証

W＆Bアカウントを認証するために、ノートブックが照会できるdatabricksシークレットを追加できます。

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

## 例

### シンプル

```python
import os
import wandb

api_key = dbutils.secrets.get("wandb", "api_key")
wandb.login(key=api_key)

wandb.init()
wandb.log({"foo": 1})
```

### スイープ

wandb.sweep（）またはwandb.agent（）を使用しようとするノートブックに必要な（一時的な）セットアップ：

```python
import os
# These will not be necessary in the future
os.environ['WANDB_ENTITY'] = "my-entity"
os.environ['WANDB_PROJECT'] = "my-project-that-exists"
```

ノートブックでスイープを実行する方法の詳細については、こちらをご覧ください。

{% page-ref page="../sweeps/python-api.md" %}

