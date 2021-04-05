# Databricks

W&B通过在[Databricks](https://www.databricks.com/)环境中定制W&B Jupyter笔记本体验，与Databricks进行集成。

## **Databricks配置** <a id="databricks-configuration"></a>

### **在集群中安装wandb** <a id="install-wandb-in-the-cluster"></a>

导航到你的集群配置，选择你的集群，点击库，然后在安装新（Install New）上,选择PyPI并添加包`wandb`

### **认证** <a id="authentication"></a>

为了认证你的W&B账户，你可以添加一个你的笔记本可以查询的datatricks secret。

```text
# install databricks clipip install databricks-cli​# Generate a token from databricks UIdatabricks configure --token​# Create a scope with one of the two commands (depending if you have security features enabled on databricks):# with security add-ondatabricks secrets create-scope --scope wandb# without security add-ondatabricks secrets create-scope --scope wandb --initial-manage-principal users​# Add your api_key from: https://app.wandb.ai/authorizedatabricks secrets put --scope wandb --key api_key
```

## **示例** <a id="examples"></a>

###  **简单** <a id="simple"></a>

```text
import osimport wandb​api_key = dbutils.secrets.get("wandb", "api_key")wandb.login(key=api_key)​wandb.init()wandb.log({"foo": 1})
```

###  **扫描（Sweep）** <a id="sweeps"></a>

试图使用wandb.sweep\(\) 或 wandb.agent\(\)的笔记本需要（临时）设置。

```text
import os# These will not be necessary in the futureos.environ['WANDB_ENTITY'] = "my-entity"os.environ['WANDB_PROJECT'] = "my-project-that-exists"
```

我们在这里介绍了更多关于如何在笔记本中运行扫描（sweep）的细节：[从Jupyter 笔记本上扫描（sweep）/sweeps/python-api](https://docs.wandb.ai/sweeps/python-api)

