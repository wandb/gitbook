---
description: 还原文件，例如模型检查点，还原至本地运行项文件夹，以供脚本使用。
---

# wandb.restore\(\)

### **概述**

调用`wandb.restore(filename)`将把文件还原至本地运行项路径。一般情况下，filename指的是之前的实验运行项生成的文件，保存在我们的云端。这一调用将把该文件复制到本地，并返回一个本地文件流，该文件流开放供读取。

 `wandb.restore`可以接受几个可选关键字参数：

* **run\_path** — s字符串型，指的是之前的运行项，要把文件复制到这一运行项，格式为“$ENTITY\_NAME/$PROJECT\_NAME/$RUN\_ID' or '$PROJECT\_NAME/$RUN\_ID”（默认值：当前的归属单位、项目名称和运行项id）
* **replace** — 布尔型，如果本地有同名文件，指定是否用云端文件覆盖本地的同名文件。（默认值：False）
* **root** —  字符串型，指定把云端文件还原至本地的哪个路径。默认值为当前工作路径，如果前面调用过wandb.init，默认值为wandb.run.dir。（默认值：“.”）

常见使用情景：

* 还原之前的运行项生成的模型架构和权值。
*  出现故障之后，从上一个检查点开始断点续训。（进入[断点续训](https://docs.wandb.ai/v/zh-hans/library/resuming)板块详细了解）

## **示例**

 若要查看完整的运行示例，请看[这篇报告](https://wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W&B--Vmlldzo3MDQ3Mw)。

```python
# restore a model file from a specific run by user "vanpelt" in "my-project"
best_model = wandb.restore('model-best.h5', run_path="vanpelt/my-project/a1b2c3d")

# restore a weights file from a checkpoint
# (NOTE: resuming must be configured if run_path is not provided)
weights_file = wandb.restore('weights.h5')
# use the "name" attribute of the returned object
# if your framework expects a filename, e.g. as in Keras
my_predefined_model.load_weights(weights_file.name)
```

> 如果你不指定run\_path，就需要为运行项配置[断点续训](https://docs.wandb.ai/v/zh-hans/library/resuming)。如果你想在训练之外用编程方式访问文件，就用[运行项API](https://docs.wandb.ai/v/zh-hans/library/import-export-api-guide)

