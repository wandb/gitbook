---
description: 把当前运行的关联文件保存至云端。
---

# wandb.save\(\)

有两种方式可以保存与运行关联的文件。

1.  用`wandb.save(filename)`；
2. 将一个文件放到 wandb运行目录下，运行结束后就会被上传。

{% hint style="info" %}
 如果你要做[断点续训](https://app.gitbook.com/@weights-and-biases/s/docs/library/resuming)，调用wandb.restore\(filename\)即可恢复关联文件
{% endhint %}

你如果想在写入文件的同时就同步文件，在`wandb.save`中指定一个文件名或通配符。

## **wandb.save示例**

 若要查看完整示例，请看[这篇报告](https://wandb.ai/lavanyashukla/save_and_restore/reports/Saving-and-Restoring-Models-with-W&B--Vmlldzo3MDQ3Mw)。

```python
# Save a model file from the current directory
wandb.save('model.h5')

# Save all files that currently exist containing the substring "ckpt"
wandb.save('../logs/*ckpt*')

# Save any files starting with "checkpoint" as they're written to
wandb.save(os.path.join(wandb.run.dir, "checkpoint*"))
```

{% hint style="info" %}
默认情况下，W&B的本地运行目录位于你的脚本的相对路径./wandb 内，路径名格式如run-20171023\_105053-3o4933r0，其中20171023\_105053是时间戳，3o4933r0是运行id。你可以将环境变量WANDB\_DIR或wandb.init的关键字参数dir，设置为绝对路径，文件就会被写入该路径。
{% endhint %}

##  **把文件保存至wandb运行项路径示例**

 文件“model.h5”被保存在wandb.run.dir，训练结束后将被上传。

```python
import wandb
wandb.init()

model.fit(X_train, y_train,  validation_data=(X_test, y_test),
    callbacks=[wandb.keras.WandbCallback()])
model.save(os.path.join(wandb.run.dir, "model.h5"))
```

 这是一个公共示例页面。你可以看到，在“文件”选项卡有个model-best.h5。这是Keras集成默认自动保存的，但你也可以手动保存一个检查点，我们会将其与你的运行关联起来为你保存。

 [查看在线示例→](https://wandb.ai/wandb/neurips-demo/runs/206aacqo/files)​

![](../.gitbook/assets/image%20%2839%29%20%286%29%20%281%29%20%285%29.png)

##  **常见问题**

###  **忽略某些文件**

 ****你可以编辑文件`wandb/settings`并设置ignore\_globs为一个逗号分割的[通配符](https://en.wikipedia.org/wiki/Glob_%28programming%29)列表。你也可以设置环境变量**WANDB\_IGNORE\_GLOBS**。一个常见的使用情景就是防止我们自动创建的git补丁被上传，**WANDB\_IGNORE\_GLOBS=\*.patch**

###   **运行结束前同步文件**

 如果你的运行时间较长，你可能希望看到模型检查点等文件在运行结束前上传到云端。而默认情况下，直到运行结束我们才上传大部分文件。那么，你可以在你的脚本中添加`wandb.save('*.pth')`或`wandb.save('latest.pth')`，以便当这些文件被写入或更新就会被上传。

###  **更改文件保存路径**

如果你默认把文件保存至AWS S3或谷歌云存储，你可能会收到这样的错误：`events.out.tfevents.1581193870.gpt-tpu-finetune-8jzqk-2033426287是一个云存储网址，无法将文件保存至wandb`

要更改TensorBoard事件文件或你希望我们同步的其他文件的记录（log）目录请将你的文件保存到wandb.run.dir以便它们会被同步到我们的云端。

**获取运行项名称**

如果你要在你的脚本中使用运行名称，你可以用`wandb.run.name`，得到运行名称——例如“blissful-waterfall-2”。你要先调用run对象的save，然后才能获得显示名称

```text
run = wandb.init(...)
run.save()
print(run.name)
```

### **把保存的全部文件推送至wandb**

 在你的脚本开头wandb.init后面调用一次`wandb.save("*.pt")`，那么，与该模式匹配的所有文件，被写入wandb.run.dir后就会立即保存。

###  **删除已同步到云存储的本地文件**

 你可以通过运行命令 wandb gc 来删除已经同步到云存储的本地文件。关于该命令的用法的更多信息，请运行\`wandb gc —help

